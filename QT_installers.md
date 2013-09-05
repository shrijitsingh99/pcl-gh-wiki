# Build a Qt installer for Windows

## Prerequisities

- Qt source archive : "4.8.0":http://download.qt.nokia.com/qt/source/qt-everywhere-opensource-src-4.8.0.tar.gz
- "Perl":http://strawberryperl.com/
- "NSIS":http://nsis.sourceforge.net/Download

## Steps

* unpack Qt sources to the build folder : e.g. C:\Qt\4.8.0 for Qt 4.8.0
**Note** The build path is hard coded in the built qmake executable, so it is highly recommended to install Qt to the same path where it was built.
* Open a Visual Studio prompt 
  - 32 bit : Start > All Programs > Microsoft Visual Studio 20XX > Visual Studio Tools > Visual Studio Command Prompt (200XX)
  - 64 bit : Start > All Programs > Microsoft Visual Studio 20XX > Visual Studio Tools > Visual Studio x64 Win64 Command Prompt (200XX)
* prompt> cd c:\Qt\4.8.0
* prompt> configure -opensource -confirm-license -fast -debug-and-release -nomake examples -nomake demos -no-qt3support -no-xmlpatterns -no-multimedia -no-phonon -no-accessibility -no-openvg -no-webkit -no-script -no-scripttools -no-dbus -no-declarative
* prompt> nmake
* prompt> nmake clean
* Zip the contents of c:\Qt\4.8.0 in a zip file (without the 4.8.0 folder)
* Open NSIS
* Choose "Installer based on ZIP file"
* In Source Zip File, click "Open" and choose the zip file you just created (it will take some time to load it)
* In Output Installer Options :
  - Installer Name  : **Qt 4.8.0 for Visual Studio 20XX winYY**, where winYY = win32 or win64
  - Default Folder  : **C:\Qt\4.8.0**
  - Output EXE File : (...)\Qt_4.8.0_msvc20XX_winYY.exe
