# Build Windows installers

This wiki explains how to build :
* Installers for Eigen, Boost, Flann, Qhull and VTK
* All-in-one installer.

To build the installers, we use a CMake script based on the ExternalProject CMake module.

The script supports :
* building and installing the dependencies and PCL (default) 
* building installers (one for each dependency + an all-in-one installer) when **BUILD_INSTALLERS** is checked.

## Prerequisites

* To build Boost MPI module, you need to install MPI from "here":http://www.mcs.anl.gov/research/projects/mpic## Choose "Win IA32 binary" if you are building 32 bit PCL libraries, or "Win X86_64 binary" if you are building 64 bit binaries. 
* To build VTK with Qt support, you need to have Qt installed.
* The script uses wget to download source archives instead of the builtin CMake functionality, because the latter doesn't support SSL protocol. If wget is not found, it is downloaded automatically on Windows.
* The script uses patch to patch the sources when needed. If patch is not found, it is downloaded automatically on Windows.

## Some notes about the script :

* The user has control over which dependencies to build.
* On Windows, most of the dependencies need to be built both in debug and release configuration **BEFORE** PCL is even configured with CMake.
* Eigen is header only, so it is only "built" in release mode.
* Boost's cmake scripts build both debug and release librairies in one run, so it's built only in release mode too.
* The script downloads source archives. This can be replaced with svn/git/hg repositories, local archives or local folders.
* A custom build command is used (pcl_build_external_project.cmake). 
* When **BUILD_INSTALLERS**=ON, the dependencies are installed to a local folder inside the build tree. Otherwise, the dependencies and PCL are installed to the default paths.
* When **BUILD_INSTALLERS**=ON, Documentation and tutorials are generated to be packed into the installer (there is no check ATM whether Doxygen, sphinx, etc.. are installed or not). 

## Instructions

* unzip attachment:[SuperBuild.zip](http://dev.pointclouds.org/attachments/download/805/SuperBuild.zip) in some folder, say C:\SuperBuild
* Open CMake gui, and configure it as if you are building a CMake based project :
  - Where is my source code : C:\SuperBuild
  - Where to build binaries : C:\SuperBuild\build
* Click "Configure", and choose the right generator.
* The script will look for wget and patch executables and download them if necessary.
* Choose the dependencies you want to built by checking/unchecking the BUILD_XXX checkboxes.
* You can customize the Boost modules to be built, by editing the **BOOST_BUILD_PROJECTS** CMake entry, which contains a * seperated list of Boost modules. If MPI is found, mpi module is appended to the list.
* if you want to build installers, then check **BUILD_INSTALLERS**, if you want to build and install the libraries on your system, then keep it unchecked.
* Click "Configure" again, then click "Generate".
* A SuperBuild.sln file is generated in C:\SuperBuild\build
* Open it and build the whole solution.
