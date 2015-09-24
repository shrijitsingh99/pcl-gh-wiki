# MacOSX

## Q: I want to use PCL in Paraview but I can't find the VTK in Paraview.

A:


A:
`error: GL/gl.h: No such file or directory
error: GL/glu.h: No such file or directory`

You have probably installed GLEW using homebrew or similar, same as in issue #465.
The workaround is to install GLEW with macports (it does not have any dependencies => just download and install macports and `sudo port install glew`) OR do a custom cmake configuration and point it manually to the location of GL/GLEW in the current system.


## Q: I am using a Kinect or Asus camera and the alignment of the RGB and depth images is off (i.e., there is a visible offset between the 3D points and the colors when I look at the cloud in `pcd_viewer`).*
As in the following image:
!rgb_depth_alignment_problems.png!

A:
Edit your `SamplesConfig.xml` configuration file. Be careful as you might have many copies of this file on your system, depending on how you installed OpenNI (I found the active one on my system here: `/etc/primesense/SamplesConfig.xml`)!
Add the following to the depth node configuration:
```<Property type="int" name="RegistrationType" value="2"/>
<Property type="int" name="Registration" value="1"/>```

So, your `SamplesConfig.xml` should look like this:

```
<OpenNI>
  <Licenses>
  </Licenses>
  <Log writeToConsole="true" writeToFile="false">
    <LogLevel value="1"/>
      <Masks>
        <Mask name="ALL" on="true"/>
      </Masks>
      <Dumps>
      </Dumps>
  </Log>
  <ProductionNodes>
    <Node type="Depth" name="Dept##>
      <Configuration>
        <Property type="int" name="RegistrationType" value="2"/>
        <Property type="int" name="Registration" value="1"/>
      </Configuration>
    </Node>
    <Node type="Image" name="Image1" stopOnError="false">
      <Configuration>
      </Configuration>
    </Node>
  </ProductionNodes>
</OpenNI>
```

Kinect works only with RegistrationType 2.
Asus works only with RegistrationType 1.

## Q: I can´t get pcl working with homebrew according to the description in the tutorial on the website.


A:
`/bin/sh: -c: line 0: syntax error '('
/bin/sh: -c: line 0: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ ...`


You have another version than the one specified in the tutorial which specifies
1.3. run `brew uninstall pcl` and run `brew install pcl --HEAD` and the change
the line `find_package(PCL 1.3 REQUIRED COMPONENTS common io)` in CMakeList.txt
to `find_package(PCL 1.8 REQUIRED COMPONENTS common io)`.