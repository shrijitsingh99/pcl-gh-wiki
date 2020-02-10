## Details

* Title: **PCL-RFC-0003: Unified API for Algorithms**
* Author: [kunaltyagi](https://github.com/kunaltyagi)
* Gitter room: [PointCloudLibrary/PCL-RFC-03](https://gitter.im/PointCloudLibrary/PCL-RFC-03)

## Motivation
Current design in several modules (filters, features, gpu) has several flaws:
* Multiple classes with independent API and independent implementations for OpenMP, GPU, CPU code
* Inextensible model for
  * adding support for thread_pools
  * multi-GPU support
  * using SIMD in OpenMP code

It is possible to use function overloading in C++ to achieve a "Unified API". This is a forward-compatible design with the proposed API for `executors` (The API is standardized since C++17, implementation is being standardized). The benefits are:
* Adding SIMD/OpenMP implementation of algorithm automatically allows the other to use it
* Ability to use thread_pools using the future model proposed by C++
* Ability to encapsulate multi-GPU support and provide it along side single-GPU support
* API remains static, allowing users to switch from CPU to OpenMP to GPU with minor changes

## Detail
A prototype with OpenMP, SIMD and CPU versions can be found [here](https://godbolt.org/#z:OYLghAFBqd5QCxAYwPYBMCmBRdBLAF1QCcAaPECAM1QDsCBlZAQwBtMQBGAFlICsupVs1qhkAUgBMAISnTSAZ0ztkBPHUqZa6AMKpWAVwC2tLgGZSW9ABk8tTADljAI0zEQ3SaQAOqBYXVaPUMTcx8/ALpbeycjV3dPRWVMVUCGAmZiAmDjU04LJRU1OnTMgmjHFzcPLwUMrJzQ/MV68rtKuOrPAEpFVANiZA4AcikzO2RDLABqcTMdZDr0LCo57HEABgBBMYmpzFn5gDcUomI1zZ3t3aoV6YB9AHkABWwHAFlny93aSYMZuZ6IzeAB0CAu10kZiseFW122SxAIBOqhIgMRIH8AC9MPcCGtpnZ8EMFPc5rJ4VsAPRU6YAFQAnt4Dpxps9iOoOQQGaRpghmAppmwCG5aMw1CcFJcRcDhCLDjoMdjcQRprQIVs6sQDKppt4OSRCAz7ngFMxydLMLLxQdARqtTrVfrOUaTWbARsCeIAOwUrbTAPTNC0OqYAAe%2BumdXFeGQUYI6CRyrx01cdXumCoNCyhwAItNJBbrt7c0WtjLvHLbfMlXgcSn1WZ1giCNrdc7Ddy3cwQHqDM5WLG9QaucbTeb5rQALScL2%2Bn2lsx%2By4GfyiNUkIxsbt54curvjhWeptl1d2YBCo5hndzfMd0c3%2BaFk9Lldri8KJSPu8j12HwGzi%2BfpnuuaYEBmWYkOB/5mD%2B%2B5ju68z3n%2BiE6Me2BImBEHZviQHfNsNL0kyBySHunYMpa1ryuiCZJnWKpqrytb1qqRi7shB6oehmGYOmmY4faraOmRD4EAgxCYMw6BlhWVYKsxDFGIJbZOr%2BXZiRJUkerySlNtMvbeP2g5xhxCETjounYLM87bIG0wgRedgisQYqsBmYYpAYZzsWpZmApZZYLmWhEsLQYDDKqCj9nUhBeQcrgEAA7pgWjTGJJFpcRgqoFQmXMgWIlGpRlY2vJtGYvRKZsbe0zoZcDq6qgwLWX6dkOYS9Citu4aed5NXcsyYpGAcpl4uJknSfM3EgE5XVuT1yBeWir62YGwahhGxDxjGcYKSmWH8VBPnwWNmmTWhaw8XxkFZIFJanu%2B0xNd49zRtomToLuz12nh2ztc99wHTdqo1d9SG%2Bd2HqXSAQMCb9VzllaJXUfMkwCoK2AeYtZwak5bI6NY9xUAYvwQJjvVopIABsUhU9MmC8njzBMeVyaRa0u4bN0LWXHZEkEAMtD44TxOkxi2aJR9gLk9jy3YBAmC9EKLNlIz2ixrx9wgsqEDdN0d2ltsePPATRMk8gEC0Ju27jrTtNq6qzPxomFUsdtWQsy7bP09o3M%2Bq1gb84LQoG98UKwncWwAGoABqXMbpuixbzBXpD1P2x1jue3RbvRh7zs5wxVh%2BzZ/qB5gAsuSHMjPsuJazJC0Lq3COzh7cmbTAwDDNlsCci%2BbECfritvp9TDvKwXrsMXnBDZ1PKbFzzq0BkHVfMAAVLX3z3Y3MItzcdxPK8HxfEb9DC2bpNg0C3hQ02V3gYdt0nqPdMM5nE97ezquT97i/%2B7zQM7UZZLS2qDYEgJr7TVhlBC4K0y4BjxhpCagpwEvWABXe4tBjCnRQbrckAZCIkCjE1A4qB0pbVYKgYAsZAEBi/kGfo58arHgDvQ1mlVVSJRIAAa0BlQ5AfDlS7gVtoaYU53YEG5rSAAHNMWkyCpJSngXZMY%2BpmDAC3E9Zq3hMhsHYKwNUODFHoAUBAExChuYSXQI6QIEA5C9jQCTKRUZ%2BTWIgMwfWy8l4ILsr/ThkjdzcOIHwgcqBBGvXotMde2j0GYJMVg4wutSB0L8WktJVhdwz1mDIaYwTQkCKEfRMs6TGHON3CbfupMQFnCRLNFy3UsagJ5iWXkTsZ68mLiUwMQVvEMPYFQIgJxiCvQ5jVfJ/DwlFJxNEtK40lHdIDE45hMgaqVMvhbK2xAtxuX/POVpE8BlDLcKMso%2BsVHl0rkLZZuE66G1bk3fA%2B8z6qnWUnCATMS5sOmKvIWbyB4wKyJDfZuY2nnLufhXu59VwnKOKaPAA5cTvM%2BT4vmFdg7/NJp40OlJZKlUBGjT80x1gnhefZJQIy4X%2BERRssmdtqb03Hp41FlyMWJwHuLEgktiDnRJfLRWYKcUIzxluOwusWUr3RVXGFlL4U0veYC6C7oQW8m4OC7euZhi9FYCAYYABWYYpBTDDA2Ia1AuqdByDkCQgYQwclmE4IaggurTV61IDwkAVMNggg2BsPVZgNgyM4DIqmMi9X5AAJy8B1cMbghqjBcF9Ual1ZrdWGoUCADYpBnUmq1aQOAsAYCIBQM9PA7AyAUAgGgYEZbqjAG9F4KgZbnIZogM4FNpBnB2EyAyXVjrSDVuGvQR4tBWC9tzaQLAW5RDsA7fgCSqRJQdoWnFDts0Y2mqEAi4gPa9BYD7U6jkCbc29BoPQJgbAOA8H4IIYQogUBWpkFu5wGbYBDQ4M4VAfhZ7DL7VTONbrUDeGKCGXVU5cxdysuB6OMdxGPFIlOJYt4JCrJkDwaY6bkipA0KI3Q%2BhciCCsBUWI8RBC%2BH8CBxoeRwgUcCMRqo7hOBJCKGkVoVHBCFFOKxso9HOiMZaGUdjTGZ68dI5wXoCh%2BiDCvdq3VBrk0TvNcMMMIapz/umMAZAcZvQglImTfAfUoRMemDfWtYCjPc0tah6QTqU1uoQBNaoySPVmAjSCCNUaqYRuDfkHgIaNhUyELquNpAE3ek4CCbg4WI2SHC1TKEVMG1eGNZupT6bM3Zrs/motEAkD9AIIZWelbq3eDM4RgzJBBBnsYCwWd17Eo7u8Ae2T%2BrDUpdTaMIzeTCAIGmCpqmanuAaa09MHTkhbMnt6A5qSTmWshYTZIGRII9WLY2A2mRZhvRUzMLF703oFOpbTYoDLObXW9Bc25jz3AvM%2BYddwfzgWY1mDax2tLmXJtBeGONg7HWJtndIMM/wGhuBAA%3D%3D%3D). [A simpler prototype](https://godbolt.org/z/fFfx3M) is also available. Please try to change the compile flags (for SSE, AVX and OpenMP) to verify that the code adapts to different choices.

The basic details are:
* A "tag" (empty struct) as the first parameter for a function call to enable overloading
* Lack of tag implies allowing PCL to choose the best option
* Tags can be inherited allowing overload resolution to choose the best option without run-time checks
* Missing implementation for a tag raises compile-time errors
* No use of macros beyond hiding the implementation for lack of supported platform. `constexpr`+`static_assert` or `SFINAE` machinery can be used to eliminate macros

For more details on implementation of executors, please see
* [Implementation of executors](https://github.com/chriskohlhoff/executors) by champion of the executor proposal
* [C++17 API reference](https://en.cppreference.com/w/cpp/algorithm/execution_policy_tag_t)
## Pros
* More freedom for user to choose the execution details
* Allows for unified API to forward the executor to C++ algorithms
* Allows reuse between SIMD, CPU and OpenMP code
* Allows single-GPU code to not pay for overhead of multi-GPU implementation
* Greater ease in testing
* Extensible for user: Bring Your Own Executors (for unsupported tags in PCL)

## Cons
* None so far except API redesign

## ABI/API Breakage
* None in the beginning. The idea is to extend current API not break it
* Complete break after deprecation

## Effort Required
Minor to Medium

## Migration Path
For implementation:
1. Add new functionality using executors
2. Original functionality will be rerouted to use the new implementation
    * **OpenMP**: use temporary tags for redirection (no executors available now, [expect proposals soon](https://link.springer.com/chapter/10.1007%2F978-3-030-28596-8_22))
    * **CUDA**: use temporary tags for redirection, and in future, [stream executors](https://github.com/henline/streamexecutordoc) (tensorflow has implementation of stream executors for CUDA and OpenCL)
3. Deprecate original functionality

For users:
* On deprecation warnings, change the class to original class, and add an executor as first argument
```
// current
pcl::NormalEstimationOMP<T1, T2> est;
est.setViewPoint(vx, vy, vz);
est.setInputCloud(cloud);
est.computePointNormal(*normal_cloud, indices, nx, ny, nz, curvature);
```
```
// proposed
pcl::NormalEstimation<T1, T2> est;
est.setViewPoint(vx, vy, vz);
est.setInputCloud(cloud);
est.computePointNormal(pcl::executor::openmp {}, *normal_cloud, indices, nx, ny, nz, curvature);
```