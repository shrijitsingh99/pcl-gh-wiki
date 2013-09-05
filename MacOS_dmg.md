# How to create DMG installers for MacOS

# install MacPorts: http://www.macports.org/install.php
# upgrade MacPorts: ```sudo port -v selfupdate```
# start building DMG installers:

  * Boost: ```sudo port -v dmg boost +universal```
  * FLANN: ```sudo port -v dmg flann +universal```
  * Eigen: ```sudo port -v dmg eigen3 +universal```
  * VTK: We still need VTK in x11 mode, as cocoa/carbon didn't work at all in our tests :(
```
sudo port -v dmg vtk5 +x11 +universal 
```
  * QHull:```sudo port -v dmg qhull +universal```
  * LibUSB-devel:```sudo port dmg libusb-devel +universal```

## How to create .pkg's for OpenNI and Sensor Libs:

The main idea of these manual steps is to replicate the _install.sh_ script that comes with the drivers.

# start off **PackageMaker** (/Developer/Applications/Utilities/PackageMaker.app)
# fill in the Install Properties
 !packagemaker_1.jpg!
# add one choice to the contents and name it OpenNI
 !packagemaker_2.jpg!
 !packagemaker_3.jpg!
# in PCL trunk (trunk/3rdparty/openni/source to be more exact), type ```make``` to create two files: *OpenNI-Bin-MacOSX-v1.3.2.1.tar.bz2* and *Sensor-Bin-MacOSX-v5.0.3.3.tar.bz2*
# from the *OpenNI-Bin-MacOSX-v1.3.2.1.tar.bz2* archive, drag the Bin, Include, Jar and Lib folders to the PackageMaker choice
 !packagemaker_4.jpg!
# go through each folder and specify its destination based on what _install.sh_ mentions (i.e., $INSTALL_BIN, $INSTALL_INC, $INSTALL_JAR). For example, our **install.sh** has:
```
INSTALL_LIB=$rootfs/usr/lib
INSTALL_BIN=$rootfs/usr/bin
INSTALL_INC=$rootfs/usr/include/ni
INSTALL_VAR=$rootfs/usr/etc/ni
INSTALL_JAR=$rootfs/usr/share/java
```
 !packagemaker_5.jpg!
**It seems that we need to have *SamplesConfig.xml* installed in /etc/openni too, so make sure to add it as shown below!**
 !packagemaker_a.jpg!
# next, create a separate bash script file and copy-paste lines from _install.sh_ that are not related to copying files (you have just done that in the previous step):
  * create database dir
  * register modules
  * Example:
```
#!/bin/sh -e
MODULES="libnimMockNodes.dylib libnimCodecs.dylib libnimRecorder.dylib"
rootfs=
INSTALL_LIB=$rootfs/usr/lib
INSTALL_BIN=$rootfs/usr/bin
INSTALL_INC=$rootfs/usr/include/ni
INSTALL_VAR=$rootfs/usr/etc/ni
INSTALL_JAR=$rootfs/usr/share/java

# make all calls into OpenNI run in this filesystem
export OPEN_NI_INSTALL_PATH=$rootfs
# make sure the staging dir OpenNI is the one being run
export LD_LIBRARY_PATH=$INSTALL_LIB

# create database dir
printf "creating database directory..."
mkdir -p $INSTALL_VAR
printf "OK\n"
mkdir -p /etc/openni
chmod -R 0777 /etc/openni
# register modules
for module in $MODULES; do
  printf "registering module '$module'..."
  $INSTALL_BIN/niReg -r $INSTALL_LIB/$module
  printf "OK\n"
done
```
# attach the script you just created to the last folder that is to be installed in your choice: Scripts tab -> Postinstall
 !packagemaker_6.jpg!
# name the package nicely and click on Build
 !packagemaker_7.jpg!
 !packagemaker_8.jpg!
# repeat the operation for Sensor - or even better, make it as a second choice
```
#!/bin/sh -e

MODULES="libXnDeviceSensorV2.dylib libXnDeviceFile.dylib"

# create file list
rootfs=
INSTALL_BIN=$rootfs/usr/bin
INSTALL_LIB=$rootfs/usr/lib
SERVER_LOGS_DIR=$rootfs/var/log/primesense/XnSensorServer

# make all calls into OpenNI run in this filesystem
export OPEN_NI_INSTALL_PATH=$rootfs
# make sure the staging dir OpenNI is the one being run
export LD_LIBRARY_PATH=$INSTALL_LIB

printf "Installing PrimeSense Sensor\n"
printf "****************************\n\n"

# register modules
for module in $MODULES; do
printf "registering module '$module' with OpenNI..."
	$INSTALL_BIN/niReg -r $INSTALL_LIB/$module $INSTALL_ETC
printf "OK\n"
done

# make server run as root
printf "setting uid of server..."
chown root $INSTALL_BIN/XnSensorServer
chmod +s $INSTALL_BIN/XnSensorServer
printf "OK\n"

# create server log dir
printf "creating server logs dir..."
mkdir -p $SERVER_LOGS_DIR
# make this dir readable and writable by all (we allow anyone to delete logs)
chmod a+w $SERVER_LOGS_DIR
printf "OK\n"

printf "\n*** DONE ***\n\n"
```
 !packagemaker_9.jpg!

## To note:

# create dmg images (should ideally have a dmg for all dependencies and one for the PCL modules):

  * Disk Utility -> New Image
# OpenNi installer: the install.sh script does not create the required /etc/openni directory which is necessary to "register" the openni modules. Also the install.sh copies files without setting their permission. Hence, this needs to be manually adjusted in the install.sh files
# SamplesConfig.xml needs to be in /etc/openni - otherwise the error "Open failed: File not found!" appears
