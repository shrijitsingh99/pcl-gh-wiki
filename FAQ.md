# FAQ

## Q: After upgrading PCL from 1.0 to 1.1, my code is crashing inexplicably!*

A: It is possible that you are using one version of the headers and a different version of the libraries. First, check your CMakeLists.txt to ensure  it is searching for the right version:
```
FIND_PACKAGE(PCL 1.1 REQUIRED)
```
This line tells CMake which headers to use. If you specify the wrong version, it will use old headers with new libraries, which is very bad.

To make things easier, if possible, `make uninstall` the old version. If not, manually delete the *include/pcl-1.0 folder*, and all .so files ending in 1.0.x.


## Q: My Qt Application exits if I use the PCLVisualizer class and close it's view window, what shall I do?*

A: This problem seems to be related to the spin()/spinOnce() function of PCLVisualizer as it registers some callbacks that might exit the whole application.
As a work around in a Qt based application you can replace the
```
while (!viewer->wasStopped())
        viewer->spinOnce(100);
```
with
```
while (!viewer->wasStopped())
        qApp->processEvents();
```
to make use of the Qt main event loop for processing the viewer events.

## Q: I want to use `pcl::NormalEstimationOMP` but setting the number of threads/cores via `setNumberOfThreads` doesn't seem to work.*

A: Try adding the `openmp` flag to your compiler, e.g.:

```
   set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fopenmp")
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fopenmp")
```
