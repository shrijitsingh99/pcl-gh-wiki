## Details

* Title: **PCL-RFC-0001: Merit in Old Ideas**
* Author: [kunaltyagi](https://github.com/kunaltyagi), [other maintainers](http://pointclouds.org/documentation/advanced/pcl2.php#pcl2)
* Gitter room: [PointCloudLibrary/PCL-RFC-01](https://gitter.im/PointCloudLibrary/PCL-RFC-01)

## Motivation

Quite a while ago, a list of changes were suggested for improvements in PCL. That list is reproduced below. It is not meant to be a TODO list but a starting point for future discussions. If any of the ideas has already been implemented, please do inform the authors.

This is a central place to track interest in these ideas. If there is sufficient interest, then more focused RFC will be created to discuss in detail.

### PointCloud
* [ ] drop templating on point types, thus making `pcl::PointCloud` template free
* [ ] drop the `pcl::PCLHeader` structure, or consolidate all the above information (width, height, is_dense, sensor_origin, sensor_orientation) into a single struct
* [ ] make sure we can access a slice of the data as a 2D image, thus allowing fast 2D displaying, [u, v] operations, etc
* [ ] make sure we can access a slice of the data as a subpoint cloud: only certain points are chosen from the main point cloud
* [ ] implement channels (of a single type!) as data holders, e.g.: 
  * `cloud[“xyz”]` => gets all 3D x,y,z data 
  * `cloud[“normals”]` => gets all surface normal data
* [ ] internals should be hidden : only accessors (begin, end …) are public, this facilitating the change of the underlying structure
* [ ] Capability to construct point cloud types containing the necessary channels at runtime. This will be particularly useful for run-time configuration of input sensors and for reading point clouds from files, which may contain a variety of point cloud layouts not known until the file is opened
* [ ] Complete traits system to identify what data/channels a cloud stores at runtime, facilitating decision making in software that uses PCL. (e.g. generic component wrappers.)
* [ ] Stream-based IO sub-system to allow developers to load a stream of point clouds and “play” them through their algorithm(s), as well as easily capture a stream of point clouds (e.g. from a Kinect). Perhaps based on Boost.Iostream
* [ ] Given the experience on [libpointmatcher](https://github.com/ethz-asl/libpointmatcher), we (François Pomerleau and Stéphane Magnenat) propose the following data structures:
  ```
  cloud = map<space_identifier, space>
  space = tuple<type, components_identifiers, data_matrix>
  components_identifiers = vector<component_identifier>
  data_matrix = Eigen matrix
  space_identifier = string with standardised naming (pos, normals, color, etc.)
  component_identifier = string with standardised naming (x, y, r, g, b, etc.)
  type = type of space, underlying scalar type + distance definition (float with euclidean 2-norm distance, float representing gaussians with Mahalanobis distance, binary with manhattan distance, float with euclidean infinity norm distance, etc.)
  ```

### PointType
* [ ] `Eigen::Vector4f` or `Eigen::Vector3f` ??
* [ ] Large points cause significant performance penalty for GPU. Let’s assume that point sizes up to 16 bytes are suitable. This is some compromise between SOA and AOS. Structures like `pcl::Normal` (size = 32) is not desirable. SOA is better in this case.

### GPU
* [ ] Containers for GPU memory. `pcl::gpu::DeviceMemory/DeviceMemory2D/DeviceArray<T>/DeviceArray2D<T>` (Thrust containers are inconvinient)
  * `DeviceArray2D<T>` is container for organized point cloud data (supports row alignment)
* [ ] `PointCloud` channels for GPU memory. Say, with “_gpu” postfix.
  * [ ] `cloud[“xyz_gpu”]` => gets channel with 3D x,y,z data allocated on GPU.
  * [ ] GPU functions (ex. `gpu::computeNormals`) create new channel in cloud (ex. “normals_gpu”) and write there. Users can preallocate the channel and data inside it in order to save time on allocations.
  * [ ] Users must manually invoke uploading/downloading data to/from GPU. This provides better understanding how much each operation costs.
* [ ] Two layers in GPU part: host layer(nvcc-independent interface) and device(for advanced use, for sharing code compiled by nvcc):
  * [ ] namespace `pcl::cuda` (can depend on CUDA headers) or `pcl::gpu` (completely independent from CUDA, OpenCL support in future?).
  * [ ] namespace `pcl::device` for device layer, only headers.
* [ ] Async operation support???

### Keypoints and Features
* [ ] The name `Feature` is a bit misleading, since it has tons of meanings. Alternatives are `Descriptor` or `FeatureDescription`.
* [ ] In the feature description, there is no need in separate `FeatureFromNormals` class and `setNormals()` method, since all the required channels are contained in one input. We still need separate `setSearchSurface()` though.
* [ ] There exist different types of keypoints (corners, blobs, regions), so keypoint detector might return some meta-information besides the keypoint locations (scale, orientation etc.). Some channels of that meta-information are required by some descriptors. There are options how to deliver that information from keypoints to descriptor, but it should be easy to pass it if a user doesn’t change anything. This interface should be uniform to allow for switching implementations and automated benchmarking. Still one might want to set, say, custom orientations, different from what detector returned.

### Data slices
* [ ] Anything involving a slice of data should use `std::size_t` for indices and not `int`. E.g the indices of the inliers in RANSAC

### RANSAC
* [ ] Renaming the functions and internal variables: everything should be named with `_src` and `_tgt`: we have confusing names like `indices_` and `indices_tgt_` (and no `indices_src_`), `setInputCloud` and `setInputTarget` (duuh, everything is an input, it should be `setTarget`, `setSource`), in the code, a sample is named: `selection`, `model_` and `samples`. `getModelCoefficients` is confusing with `getModel` (this one should be `getBestSample`).
* [ ] no const-correctness all over, it’s pretty scary: all the get should be `const`, `selectWithinDistance` and so on too.
* [ ] the `getModel`, `getInlier`s function should not force you to fill a vector: you should just return a const reference to the internal vector: that could allow you to save a useless copy
* [ ] some private members should be made protected in the sub sac models (like sac_model_registration) so that we can inherit from them.
* [ ] the `SampleConsensusModel` should be independent from point clouds so that we can create our own model for whatever library. Then, the one used in the specialize models (like sac_model_registration and so on) should inherit from it and have constructors based on `PointClouds` like now. Maybe we should name those `PclSampleConsensusModel` or something (or have `SampleConsensusModelBase` and keep the naming for `SampleConsensusModel`).