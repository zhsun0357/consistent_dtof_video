## DyDToF Synthetic Dataset

This dataset is generated with Unreal Engine 4.27.2 and publicly available 3D assets (including environmental maps, meshes, and animations). 
The dataset contains 100 videos, with 45k frames in total. These videos are generated from 16 indoor environments and \~50 animal animations, with diverse environments, photo-realistic rendering and dynamical objects. We use the [EasySynth] plugin to generate most of the data.

Disclaimer: part of the animal motions and interactions with the environment might not make physical sense (e.g., a horse is running but not moving with correct speed).

[EasySynth]: https://github.com/ydrive/EasySynth

### Dataset Overview

For each video, we provide 5 data modalities for now: 
+ color images (in .jpeg format)
+ depth maps (in .npy format)
+ surface normal maps (in .npy format)
+ albedo map (in .png format)
+ camera trajectories (in .csv format)

We are planning to update our dataset with stereo/multi-view and other data modalities in the future.

The dataset is organized as follows:

	scene1                                              # scene
	|													
	--- sequence1                                       # video sequence
	|       --- ColorImage                              
	|       |      |
	|       |      ---- <seq_name>.<index>.jpeg         # color image
	|       |      |      |
	|       --- DepthMap                              
	|       |      |
	|       |      ---- <seq_name>.<index>.npy          # depth map
	|       |      |      |
	|       --- SurfaceNormal                             
	|       |      |
	|       |      ---- <seq_name>.<index>.npy          # surface normal map
	|       |      |      |
	|       --- AlbedoImage                              
	|       |      |
	|       |      ---- <seq_name>.<index>.png          # albedo image
	|       |      |      |
	|       --- CameraPoses.csv                         # camera trajectory
	.       .      .      .
	.       .      .      .


### Depth, Surface Normal and Camera Pose Conventions
**Depth**: The depth values are in unit of meters and are clipped to be within 0-50 meters range (at window pixels, the depth values are 50 meters). When used as input to a depth prediction algorithm, you can scale depth values in a video clip according to the depth value distribution within that clip. You also need to scale the camera translation vector accordingly if multi-view geometry is used.

NOTE: we scaled the depth values to be consistent with the scale of camera translation vector to simplify back-projecting depth values into 3D space. We provide a [script] for generating back-projected point-cloud from camera pose and depth map.

**Pose**: Each line in the camera trajectory csv file has the following format:

| Column | Type  | Name | Description                |
| ------ | ----- | ---- | -------------------------- |
| 0      | int   | id   | 0-indexed frame id         |
| 1      | float | tx   | X position in meters 		 |
| 2      | float | ty   | Y position in meters 		 |
| 3      | float | tz   | Z position in meters 		 |
| 4      | float | qw   | Rotation quaternion W      |
| 5      | float | qx   | Rotation quaternion X      |
| 6      | float | qy   | Rotation quaternion Y      |
| 7      | float | qz   | Rotation quaternion Z      |
| 8      | float | fx   | Focal Length X             |
| 9     | float | fy   | Focal Length Y              |
| 10     | float | cx   | Camera Center X            |
| 11     | float | cy   | Camera Center Y            |


NOTE: The coordinate system used in the csv file and in Unreal Engine are shown in the following figure. Both are **left handed** coordinates. The absolute values of translations are 100x larger in the Unreal Engine GUI.

<img src="https://zhsun0357.github.io/consistent_dtof_video/figures/ue_cam_coord.png" style="margin-right: 10px; width: 500px"/>

[script]: https://drive.google.com/drive/folders/1PNKEelbscx6_UL7YDH8LXt2Pq4x-qFea?usp=sharing/

**Surface Normal**: Surface normals are in the camera coordinate, and have L2 norm equals 1

NOTE: at window pixels, the generated raw surface normal vectors are not well-defined and do not have correct L2 norms, so we manually normalized them. 

### Citation

This dataset comes with the CVPR 2023 paper "Consistent Direct Time-of-Flight Video Depth Super-Resolution". If you find our dataset to be helpful, please consider citing:

	@article{sun2022consistent,
	  title={Consistent Direct Time-of-Flight Video Depth Super-Resolution},
	  author={Sun, Zhanghao and Ye, Wei and Xiong, Jinhui and Choe, Gyeongmin and Wang, Jialiang and Su, Shuochen and Ranjan, Rakesh},
	  journal={arXiv preprint arXiv:2211.08658},
	  year={2022}
	}

