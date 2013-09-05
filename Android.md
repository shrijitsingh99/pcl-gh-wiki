## How to compile PCL for Android

## Step 1: install the Android SDK and NDK

  * http://developer.android.com/sdk/index.html
  * http://developer.android.com/sdk/ndk/index.html

After this, you might want to follow the superbuild method, discussed here:
http://www.pcl-users.org/pcl-for-android-td4022486.html
rather than following the steps below which seem to be out-of-date. Also, ndk-r8b is
now supported by the superbuild (as of 2012-10-05, thanks Pat).


## Step 2: create an independent toolchain for the Android NDK. You can do this by using the script:

 `/work/PCL/Android/android-ndk-r6b/build/tools/make-standalone-toolchain.sh --ndk-dir="/work/PCL/Android/android-ndk-r6b/" --install-dir="/work/PCL/Android/android-9-toolchain" --platform="android-9"
`

```
 export ANDROID_SDK=/work/PCL/Android/android-sdk-linux_x86/
 export ANDROID_NDK=/work/PCL/Android/android-ndk-r6b/
 export ANDROID_NDK_TOOLCHAIN_ROOT=/work/PCL/Android/android-9-toolchain
 shopt -s expand_aliases
 export PATH=$PATH:$ANDROID_SDK/tools:$ANDROID_NDK
 
 export ANDROID_CMAKE=/work/PCL/pcl/trunk/android/android_dependencies/android-cmake
 export ANDTOOLCHAIN=/work/PCL/pcl/trunk/android/android_dependencies/android-cmake/toolchain/android.toolchain.cmake
 alias android-cmake='cmake -DCMAKE_TOOLCHAIN_FILE=/work/PCL/pcl/trunk/android/android_dependencies/android-cmake/toolchain/android.toolchain.cmake -DCMAKE_INSTALL_NAME_TOOL=/usr/bin/install_name_tool '
```


## Step 3: compile and install dependencies for PCL

```
pcl_trunk$ cd android/android_dependencies
pcl_trunk/android/android_dependencies$ ./install_dependencies.sh
```


## Step 4: compile PCL

```
pcl_trunk$ mkdir build_android && cd build_android
pcl_trunk/build_android$ android-cmake -DANDROID_API_LEVEL=9 -DCMAKE_INSTALL_PREFIX=/work/PCL/Android/android-ndk-r6b/toolchains/arm-linux-androideabi-4.4.3/prebuilt/linux-x86/user/ -DBUILD_visualization=OFF -DBUILD_global_tests=OFF -DBUILD_people=OFF ..
```
