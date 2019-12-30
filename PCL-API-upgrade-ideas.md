# WIP; Dragons be Here
Inkomplete work and speling errors also frolic with the dragons

# Introduction

PCL 1.10 is about to be released, and has a massive changelog. With new patches coming in, there have been requests to upgrade PCL's API to make it more developer friendly.

The time of the maintainers is however limited, and as such we need to prioritize the changes, as well as balance the community's expectations of a stable API vs better interface. This document is being created as one place to
* provide an overview to the community of things to come
* provide a central place for maintainers regarding future direction
* provide a starting place for **fruitful** discussions
* manage the expectations vs reality

This document is a living document and will be updated as and when required. Please note that this is **not a formal document**, and has been created by [Kunal Tyagi] for his personal role as of now. This might change in the future.

## Requests
* Please show support for both how valid/invalid a certain proposal is as well as which "flavor" of the proposal is better
* Please keep the discussions polite and respectful
* Please refrain from asking questions like "how long it'll take"

## Disclaimer
Currently, I'm personally involved in upgrading the internals of PCL to use `std::size_t` as the basic type for array indices (and upgrade to using algorithms where possible). I hope the migration ends up being a success, even though `std::size_t` isn't the best type to use, it felt the best candidate at that time. The discussions pointed out several shortcomings and I hope to avoid them in the future with another proposal of mine: `executors`

# Current Proposals

| Proposal | Status | Proposer |
|----|----|----|
| [Reduce usage of `std::shared_ptr<T>` in the API](Reduce-usage-of-stdshared_ptrT-in-the-API) | ❓ | Community |
| [Better type for indices](better-type-for-indices) | ❓ | Community and Maintainers |
| [Combine PointCloud with acceleration data-structures](Combine-PointCloud-with-acceleration-data-structures) | ❓ | Community |
| [`executors` for One-API-for-Algorithms](executors-for-One-API-for-Algorithms) | ❓ | [Kunal Tyagi] |
| [Python API](#python-api) | ❓ | [Kunal Tyagi] |
| [Some build dependencies](some-build-dependencies) | ❓ | [Kunal Tyagi] |
| [Fluent style API for algorithms](Fluent-style-API-for-algorithms) | ❓ | [Kunal Tyagi] |
| [Ranges and Co-routines for algorithms](Ranges-and-Co-routines for algorithms) | ❓ | [Kunal Tyagi] |

# Proposal Details
## Reduce usage of `std::shared_ptr<T>` in the API
Current API relies heavily on `std::shared_ptr`. Instead we can simply use references to the data-type or a pointer if `nullptr` is a valid input.

### Original rationale
Smart pointers and move-semantics came to C++ pretty late, and thus Boost.SharedPtr needed to be used. Sadly, this meant that current use of `std::shared_ptr` includes usage of:
* potential `unique_ptr`
* valid use of `shared_ptr`
* over-zealous use of `shared_ptr` where naked_ptr (or naked_ref) would be better since the resource was anyways in shared_ptr

The idea is to keep the second usage of `shared_ptr`. The fist type of usage has the `assert(ptr.use_count() == 1)`. The third type of usage is signified by presence of
* `const std::shared_ptr<T>&`
* no mutex/synchronization mechanism (for thread safety)
* assumption that the lifetime of function/object will be smaller than that of the `shared_ptr`

### Pros
* Follows C++ guidelines: Better transmission of intent since using raw pointer implies algorithm doesn't **own** the resource
* Might result in a better ownership model in the library
* Algorithms work with stack-based point clouds too
* No `nullptr` checks needed
### Cons
* PointCloud may go out-of-scope while the algorithm/tree-query is in progress: Technically *none-of-our-concern* since the user needs to ensure this

### Migration Path
For the users, copy-and-pin technique removes most of the issues that might arise due to this proposal. This technique is **only required** when 
* calling PCL functions that take `const std::shared_ptr<T>&`
* Lifetime of the pointer might be in question

If none of this is of concern, then the first line can be skipped
```c++
// current
pcl::Algorithm obj;
obj.setPointCloud(point_cloud);
```
```c++
// future
std::shared_ptr cloud = point_cloud;  // point_cloud is a reference to shared_ptr
pcl::Algorithm obj;
obj.setPointCloud(cloud.get());  // proceed with the raw pointer
```

## Better type for indices
### Original rationale
Currently, there's mixed use of `int`, `long`, `unsigned int`, `unsigned long`, `unsigned long long` and `std::size_t` for indices. Some effort has been made to unify towards `std::size_t` to reduce the limit of 2 billions points due to `int`

### Pros
Follows a best practice

### Cons
* One of the following complication:
  * Extra build-time dependency on internet
  * Copy-pasted file
  * `git` sub-tree/sub-module
* `gsl::index` is just `std::ptrdiff_t` (but there's already `std::ssize_t` so maybe there's a better type out there)

### Migration Path
None needed. It might help to have a `pcl::index` but that's not the main concern

### Question
Is there anyone who has been adversely impacted with `std::size_t` instead of `int`?

## Combine PointCloud with acceleration data-structures
Consider the new PointCloud struct to be the following:
```c++
struct SmarterIndices {
std::unique_ptr<Indices> included;
std::unique_ptr<Indices> excluded;
std::unique_ptr<Indices> delta_removed;
};

struct FilterOptions;  // like keepOrganized, extractNegative, etc.

struct CloudDetails {
std::shared_ptr<CurrentPointCloud> cloud;
std::shared_ptr<Tree> acceleration_tree;  // computed lazily
std::shared_ptr<SmarterIndices> indices;  // computed lazily
};
// one cloud for many acceleration data structures
// one acceleration data structure and cloud for many indices

struct PointCloud {
std::shared_ptr<CloudDetails> details;
};
```

Compare the following code snippets:
```c++
pcl::StatisticalOutlierRemoval<pcl::PointXYZ> sor;
sor.setMeanK (50);
sor.setStddevMulThresh (1.0);
// -------------- //
sor.setIndices (indices);
sor.setInputCloud (cloud);
sor.filter (*cloud_filtered);
sor.getRemovedIndices (removed_indices);
```
```c++
pcl::StatisticalOutlierRemoval<pcl::PointXYZ> sor;
sor.setMeanK (50);
sor.setStddevMulThresh (1.0);
// -------------- //
cloud_filtered = sor.setExtractRemovedIndices (true).filter (cloud);
// -------------- //
auto options = FilterOptions ().setExtractRemovedIndices (true);
cloud_filtered = sor.filter (cloud, options);
```
### Benefits
* Simplifies and integrates the API for algorithms: Just pass the algorithm specific options and this struct (by copy/value/reference?) and get this struct as output
* Makes it easy to follow guideline: Have a const PointCloud and work on indices
* Makes it easy to ensure that the acceleration tree and the point cloud are "in sync". (personal anecdote: forgot to update the tree in a video-pipeline)
* Paves way to create helper acceleration data-structure which work for "truncated/transformed copies" of the original cloud
* Integration with Fluent Interface will result in smaller, more legible code

### Cons
* Copy cost of `shared_ptr` is not zero (or even negligible)
* Makes it easy to hide cost of updating the tree and indices (can be offset by creating options in the algorithm to make the cost explicit)

## `executors` for One-API-for-Algorithms
Currently, it's not possible to have one algorithm with multiple implementations across OpenMP, GPU, CPU, thread_pool, etc. This can be made possible with executors. Executors are a new concept, as such here's a list of resources:
* [Nice Library by @chriskohlhoff](https://github.com/chriskohlhoff/executors)
* [C++ reference](https://en.cppreference.com/w/cpp/algorithm/execution_policy_tag_t)

**TL;DR for laymen**: Adding a "tag" as the first argument can help in using the same class choose the correct implementation. C++ extends this by allowing the "tag" to carry more information about the execution context, such as which thread_pool to use, how many threads, which device, etc.

## Python API
A Python API has unique challenges for a library which depends heavily on templates like PCL. However, since Python expects higher abstractions than C++, a Python API can tell the maintainers when something needs a "more beautiful syntax".

This is not a priority as per me, more of a guiding tool in modifying the API. It also creates the core-PCL as a project that needs to be used (or rather wrapped) highlighting flaws of the design which aren't visible when developing and not using the project.

## Some build dependencies
We should consider adding some build dependencies in order to prepare for future C++ versions. Since we are currently in C++14, next target release would be C++20 or C++23. As such we should design and develop with the future features in mind, but use libraries to offset the gap between current and target stdlib capabilities.

* [GSL](https://github.com/microsoft/GSL): Follow C++ best practices
* [Abseil](https://github.com/abseil/abseil-cpp): Nifty additional containers, specially for added speed/consistent time usage over stdlib ones, and C++17 features in C++14 compilers
* [Ranges v3](https://github.com/ericniebler/range-v3): Better algorithms, coming in C++20, C++23
* [CppCoro](https://github.com/lewissbaker/cppcoro/): C++20 lacks generators, scheduled in C++23
* [Executors](https://github.com/chriskohlhoff/executors): scheduled for C++23
* **Optional** [Hip](https://github.com/ROCm-Developer-Tools/HIP) or [HipSYCL](https://github.com/illuhad/hipSYCL): GPU code for Nvidia + AMD devices

This is on top of current dependency on system installed Boost, Eigen, Flann and VTK. The difference between the existing dependencies and proposed dependencies is that PCL will manually manage them. It is expected that there will be a detailed discussion regarding merits and demerits of each and every single proposed dependency.

### Pros
* Development can start soon on future features

### Cons
* CMake wizardry required (PoC is ready for header only libraries)
* Compile times might increase for old compilers (increases in run-time speed (due to coroutines, ranges) and better API might offset them. Needs discussion)

### Migration
None for the users

## Fluent style API for algorithms
Compare the following snippets of code
```c++
pcl::StatisticalOutlierRemoval<pcl::PointXYZ> sor;
sor.setMeanK (50);
sor.setStddevMulThresh (1.0);
sor.setIndices (indices);
sor.setInputCloud (cloud);
sor.filter (*cloud_filtered);
sor.getRemovedIndices (removed_indices);
```
```c++
const auto result = pcl::StatisticalOutlierRemoval<pcl::PointXYZ>  // algo
                    .setMeanK (50).setStddevMulThresh (1.0)        // options
                    .setInputCloud (cloud).setIndices (indices)    // inputs
                    .filter (*cloud_filtered).getRemovedIndices (removed_indices)  // outputs
                    .result();
```

Apart from utilizing the horizontal space, fluent interface enable chaining in order to hide the return value and extend OOP capability of the library. Example:
```c++
// not valid PCL code
const auto result = pcl::FilterOptions().setExtractRemovedIndices(true)
                    .useAlgorithm(user_input_based_tag).algo().options(user_input_based_options)
                    .setInputCloud (cloud).setIndices (indices)
                    .filter (*cloud_filtered).getRemovedIndices (removed_indices).result();
```
Here, the return type changed from `FilterOptions` to `pcl::Filter` to `bool`. These are straw-men examples and are chosen to showcase the difference between the 2 styles of interface. Do note that old code is still valid code.

## Pros
* Integrates very well with ranges

## Cons
* Requires a strict hierarchy and SOLID implementation for each class

### Migration
None

## Ranges and Co-routines for algorithms
TODO

## End-Of-List
---
[Kunal Tyagi]: https://github.com/kunaltyagi