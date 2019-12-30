# WIP; Dragons be Here

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
| Reduce usage of `std::shared_ptr<T>` in the API | ❓ | Community |
| Use `gsl::index` instead of `std::size_t` for indices | ❓ | Community and Maintainers |
| [`executors` for One-API-for-Algorithms](executors-for-One-API-for-Algorithms) | ❓ | [Kunal Tyagi] |
| [Python API](#python-api) | ❓ | [Kunal Tyagi] |

# Proposal Details
## Python API
A Python API has unique challenges for a library which depends heavily on templates like PCL. However, since Python expects higher abstractions than C++, a Python API can tell the maintainers when something needs a "more beautiful syntax".

This is not a priority as per me, more of a guiding tool in modifying the API. It also creates the core-PCL as a project that needs to be used (or rather wrapped) highlighting flaws of the design which aren't visible when developing and not using the project.

## `executors` for One-API-for-Algorithms
Currently, it's not possible to have one algorithm with multiple implementations across OpenMP, GPU, CPU, thread_pool, etc. This can be made possible with executors. Executors are a new concept, as such here's a list of resources:
* [Nice Library by @chriskohlhoff](https://github.com/chriskohlhoff/executors)
* [C++ reference](https://en.cppreference.com/w/cpp/algorithm/execution_policy_tag_t)

**TL;DR for laymen**: Adding a "tag" as the first argument can help in using the same class choose the correct implementation. C++ extends this by allowing the "tag" to carry more information about the execution context, such as which thread_pool to use, how many threads, which device, etc.
---
[Kunal Tyagi]: https://github.com/kunaltyagi