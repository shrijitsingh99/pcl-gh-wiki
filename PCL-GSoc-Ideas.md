# Ideas for GSoC

## Idea List

### Project Maintenance
* Website
* Tutorials
  * Update to reflect forward movement in PCL
  * Merge with documentation to make a single website
* Documentation
  * Improve documentation coverage
  * Migrate to sphinx
  * (Stretch goal) Add CI test for missing documentation in "new code"

### Quality of Life
* Benchmarks and Performance monitoring
  * Nightly CI jobs to measure performance on `master`
  * (Stretch goal) Incremental bench-marking on PR
* Compilation time reduction
  * Refactoring/CMake changes/Sub-Modules to reduce compilation time
  * 100% reproducible incremental builds (with a constant config)
  * (Stretch goal) Compile time reduction on CI using
    * incremental builds
    * compiler caches
* Improving confidence in builds
  * Incremental builds on CI
  * More warnings and sanitizers for CI
  * Integrate `clang-tidy`, static-analyzers
  * Increasing test coverage

### Code Maintenance
* Refactoring and Modernization (underway)
  * Expand ideas here

### New ideas
??

## Resources
### Ideas Pages
If any of the following ideas are not implemented, please notify [Kunal Tyagi](https://github.com/kunaltyagi) on Gitter using [PM](https://gitter.im/kunaltyagi) or [PCL chatroom](https://gitter.im/PointCloudLibrary/pcl). They will be added to the ideas list.
* [Discussion on Ideas for GSoC 2011](http://www.pcl-developers.org/two-more-projects-for-GSOC-tt4645184.html#none)
* [Ideas page for GSoC 2011](https://web.archive.org/web/20130314145536/http://www.pointclouds.org:80/gsoc2011/ideas.html)
* [Ideas page for GSoC 2014](http://www.pointclouds.org/gsoc/)
### Project Pages
If any of the following projects are not integrated in PCL, please notify [Kunal Tyagi](https://github.com/kunaltyagi) on Gitter using [PM](https://gitter.im/kunaltyagi) or [PCL chatroom](https://gitter.im/PointCloudLibrary/pcl). They will be slated for review and addition in PCL-core (if maintainers are found) or PCL contrib (like opencv-contrib) (if there is a lack of maintainers)
* [GSoC 2011](http://www.pointclouds.org/blog/gsoc/)
* [GSoC 2012](https://web.archive.org/web/20121009031358/http://pointclouds.org:80/news/pcl-gsoc-kickstart.html)
* [GSoC 2014](http://www.pointclouds.org/blog/gsoc14/index.php)