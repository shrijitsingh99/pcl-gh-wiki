<https://github.com/PointCloudLibrary/pcl/commit/eb3cb6a4f13f16c81315b6bc1b36db5704ff3bfc> fixed a bug in the viewpoint generation code for pcd files. In case you point cloud seems to be flipped after this commit, you can simply fix this using:

`sed -i 's/VIEWPOINT 0 0 0 0 1 0 0/VIEWPOINT 0 0 0 1 0 0 0/'`

or by editing the header directly. More can be found in:
* https://github.com/PointCloudLibrary/pcl/issues/163
* https://github.com/PointCloudLibrary/pcl/issues/116