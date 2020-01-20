# Ideas for GSoC
 
## 2020 Idea List
PCL returns to GSoC after a long hiatus. This year, PCL is looking for contributions and active community members from all corners of the coding community in the roles of
* technical writers
* designers
* web developers
* C++ developers
* CI/CD experts
* polyglots
* wizards
* mentors

and everything else in between.

### Project Maintenance (`proj`)
As the community grows, soft-skills and community outreach become more important. With the change in community, it has become important to update the overlooked facet of PCL and make it more accessible to the community
1. Update outdated Website
    * Create a static website on Github pages or similar
    * Provide a basic replacement including:
      * documentation for past releases
      * prevent invalidation of links as much as possible, while updating the content
    * Make it the new home for `pointclouds.org` with HTTPS support
    * (Stretch goal) Migrate all content from existing website, including 
      * ability to create blog posts (using Jekyll or similar)
      * lost content (from internet archives)
2. Walk through documentation (Tutorials, Guides and Examples)
    * Update to reflect forward movement in PCL
    * Merge with auto-gen API documentation to make a single website
    * Detect and increase coverage
3. Reference Manual Documentation
    * Improve documentation coverage
    * Migrate front-end to sphinx
    * (Stretch goal) Add CI test for missing documentation in "new code"

### Quality of Life (`QoL`)
As PCL has matured, it has discovered missing features: features that are more than just the core code, features that are vital for delivering continuous improvements. We look forwards for the ideas/projects with a minor or negligible addition of "code features" as output. Some of the wish-list features are:
1. Benchmarks and Performance monitoring
    * Nightly CI jobs to measure performance on `master`
    * (Stretch goal) Incremental bench-marking on PR
2. Compilation time reduction
    * Refactoring/CMake changes/Sub-Modules to reduce compilation time
    * 100% reproducible incremental builds (with a constant config)
    * (Stretch goal) Compile time reduction on CI using
      * incremental builds
      * compiler caches
3. Improving confidence in builds
    * Incremental builds on CI
    * More warnings and sanitizers for CI
    * Integrate `clang-tidy`, static-analyzers
    * Increasing test coverage
    * ABI/API breakage monitoring for PR
4. Make a better CMake
    * More DRY, less wizardry
    * Automatic module discovery
    * Automatic test discovery (refactoring test code layout is ok)
    * Out-of-source PCL-contrib super-module similar to OpenCV-contrib

### Code Maintenance (`code`)
Old API designs result in greater friction between how developers prefer to use vs how PCL lets developers use the API. Overhauling API used to be difficult but with `libtooling` and `clang-tidy`, it has been made manageable. Some of the ideas for guiding PCL towards a more modern API are:
1. Refactoring and Modernization
  * [Better type for indices](https://github.com/PointCloudLibrary/pcl/wiki/PCL-RFC-0002:-Better-type-for-indices)
  * Fluent style API for algorithms
  * Extend `format` and `clang-tidy --fix` to other modules

### New features (`new`)
PCL is missing a lot of state-of-the-art implementations. Suggestions/implementations of such algorithms are highly welcome since highly performant, cutting edge algorithms are a core component of PCL. Apart from them, the following features are also on the wish-list of PCL:
1. Binding interface for (Python, Matlab/Octave, C) (1 project per wrapping isn't worth 3 months)
    * Needs auto-discovery (like [binder](https://github.com/RosettaCommons/binder))
      * Reduce drift between core code and bindings
      * Reduce maintenance burden
    * Tests
    * Existing wrappers for Python for inspiration
      * [pcl-py](https://github.com/strawlab/python-pcl)
      * [pypcl](https://github.com/davidcaron/pclpy)
    * (Stretch goal) Support different PCL releases and Language versions
2. [Unified API for PCL algorithms](https://github.com/PointCloudLibrary/pcl/wiki/PCL-RFC-0003:-Unified-API-for-Algorithms)

## Contact
For interested students:
* Please announce yourself and your interest in PCL at the [gitter chat](https://gitter.im/PointCloudLibrary/pcl) 
* Do not contact the mentors privately
* As per your interest, you'd be assigned a small task to evaluate your skills

For those interested in becoming mentors:
* Please note that mentoring and co-mentoring can take a lot of effort from your side (from 10 hours a week or 1 hour a week depending on the project, the student, the problems, etc.)
* Please announce yourself and participate in discussions at the [gitter chat](https://gitter.im/PointCloudLibrary/pcl)
* Feel free to PM existing maintainers/mentors
* "Teaming up" as co-mentors is an option if you feel you will not be able to be the sole mentor on a project. For projects with existing mentor(s), it is upto them to accept/reject a co-mentor

### Mentors
Alphabetical list of mentors
* kunaltyagi
* taketwo

## Resources
### Ideas Pages
If any of the following ideas are not implemented, please notify [kunaltyagi](https://github.com/kunaltyagi) on Gitter using [PM](https://gitter.im/kunaltyagi) or [PCL chatroom](https://gitter.im/PointCloudLibrary/pcl). They will be added to the ideas list.
* [Discussion on Ideas for GSoC 2011](http://www.pcl-developers.org/two-more-projects-for-GSOC-tt4645184.html#none)
* [Ideas page for GSoC 2011](https://web.archive.org/web/20130314145536/http://www.pointclouds.org:80/gsoc2011/ideas.html)
* [Ideas page for GSoC 2014](http://www.pointclouds.org/gsoc/)
### Project Pages
If any of the following projects are not integrated in PCL, please notify [kunaltyagi](https://github.com/kunaltyagi) on Gitter using [PM](https://gitter.im/kunaltyagi) or [PCL chatroom](https://gitter.im/PointCloudLibrary/pcl). They will be slated for review and addition in PCL-core (if maintainers are found) or PCL contrib (like opencv-contrib) (if there is a lack of maintainers)
* [GSoC 2011](http://www.pointclouds.org/blog/gsoc/)
* [GSoC 2012](https://web.archive.org/web/20121009031358/http://pointclouds.org:80/news/pcl-gsoc-kickstart.html)
* [GSoC 2014](http://www.pointclouds.org/blog/gsoc14/index.php)