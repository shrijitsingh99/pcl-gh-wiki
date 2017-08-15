
# Shipping Binaries
## Mac OSX
Ship only modules enabled by default. Assume only the following optionals: existence of VTK and OpenGL support

* Go to CMakeLists.txt and comment out all section regarding RPATH setup (we should allow this to be specified with CMAKE).
* Configure
```shell
$ cmake .. -DCMAKE_INSTALL_PREFIX=./release \
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
$ make -j install
# and pray your system doesn't run out of memory