## Details

* Title: **PCL-RFC-0002: Better type for indices**
* Author: [kunaltyagi]
* Gitter room: [PointCloudLibrary/PCL-RFC-02](https://gitter.im/PointCloudLibrary/PCL-RFC-02)
* Past discussion: [#3351](https://github.com/PointCloudLibrary/pcl/issues/3351)
  * People of Interest: [jasjuang], [aPonza], [taketwo], [SergioRAgostinho]

[kunaltyagi]: https://github.com/kunaltyagi
[jasjuang]: https://github.com/jasjuang
[aPonza]: https://github.com/aPonza
[SergioRAgostinho]: https://github.com/SergioRAgostinho
[taketwo]: https://github.com/taketwo

## Motivation
Currently, there's mixed use of `int`, `long`, `unsigned int`, `unsigned long`, `unsigned long long` and `std::size_t` for indices. Some effort has been made to unify towards `std::size_t` to reduce the limit of 2 billions points due to `int`.

## Pros
* Follows a best practice
* Allows operations on actually limitless points (if using 64 bit systems)

## Cons
* One of the following complication if using `GSL`
  * Extra build-time dependency on internet
  * Copy-pasted source file
  * `git` sub-tree/sub-module
* A new typedef `using pcl::index_t = std::ptrdiff_t;`
* Not platform independent
* Wastes memory: Largest RAM supported is ~32TB (2^48). That's a clear 25% loss of efficiency for extreme cases, and larger for average use-case.

## ABI/API Breakage
* None for existing internal migration
* Major API breakage for changing `Indices` from `std::vector<int>` to `std::vector<index_t>`

## Effort Required
Minor to medium, split over multiple phases:
1. Internal API modification is underway, would need modification from `std::size_t` to `pcl::index_t`
2. Modification of public API will be faster since it is easily identifiable (function declaration, data members)

## Migration Path
Multi-step migration:
1. Replace `std::vector<int>` with `IndicesVec`
2. Replace `std::size_t` with `pcl::index_t`
3. Replace `IndicesVec = std::vector<int>` with `IndicesVec = std::vector<pcl::index_t>`

## Question
Is there anyone who has been adversely impacted with size increase and cache misses due to `std::size_t` instead of `int`? If so, there might be merit in discussing alternatives to `std::ptrdiff_t` (such as `std::int32_t` instead, which is the same but platform independent and brings back performance)