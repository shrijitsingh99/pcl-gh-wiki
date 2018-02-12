# Task list
- [ ] Go through all PR for the milestone
- [ ] Update Changelog
- [ ] Start a release branch
- [ ] (If needed) Revert whatever unintentional API/ABI breakage that might have occurred in the release branch
- [ ] Bump version to x.x.x in CMakeLists.txt
- [ ] Find replace all occurrences of x.x.x.99 CMake requirements in the tutorials and examples
- [ ] Try to build and test on all relevant platforms
- [ ] Finalize changelog and fix the release date
- [ ] Tag
- [ ] Remove the release branch
- [ ] Add binaries to the GitHub release page.
- [ ] Add link doc link to tutorials page
- [ ] Bump version to x.x.x-dev in CMakeLists.txt
- [ ] Send announcements to the mailing lists
- [ ] Throw a party!

# Shipping Binaries
## Mac OSX
Ship only modules enabled by default. Assume only the following optionals: existence of VTK and OpenGL support

* Go to CMakeLists.txt and comment out all section regarding RPATH setup (we should allow this to be specified with CMAKE).
* Configure
```shell
$ cmake .. -DCPACK_GENERATOR="TBZ2" \
           -DCPACK_PACKAGE_FILE_NAME="pcl-1.x.x-darwin"
           -DCMAKE_BUILD_TYPE=Release \
           -DPCL_ENABLE_SSE=OFF \
           -DWITH_CUDA=OFF \
           -DWITH_DAVIDSDK=OFF \
           -DWITH_DOCS=OFF \
           -DWITH_DSSDK=OFF \
           -DWITH_ENSENSO=OFF \
           -DWITH_FZAPI=OFF \
           -DWITH_LIBUSB=OFF \
           -DWITH_OPENGL=ON \
           -DWITH_OPENNI=OFF \
           -DWITH_OPENNI2=OFF \
           -DWITH_PCAP=OFF \
           -DWITH_PNG=OFF \
           -DWITH_QHULL=OFF \
           -DWITH_QT=OFF \
           -DWITH_RSSDK=OFF \
           -DWITH_VTK=ON
$ make -j package
# and pray your system doesn't run out of memory