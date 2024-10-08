# Appras_Bearcar_24

This repository is based on the [Issac-ROS](https://developer.nvidia.com/isaac/ros) framework. The objective of this package is to integrate Isaac ROS Visual SLAM on the Bearcar while assessing its efficacy and robustness of the algorithm within the environment.

| <img  src="./bearcar/gifs/frames.gif"> | <img  src="./bearcar/gifs/keypoints.gif"> | 
| :--------------------------------------------------------------: | :---------------------------------------------------------------------: | 

## Requirements

1. Jetson Xavier AGX 
2. At least 128GB SD Card (SSD recommended)
3. Intel Realsense D455 (D435i)
4. URDF of the Robot 
5. Jetpack 5.1.2

## How to use ?

### Initial Setup

Create a workspaace and clone the repository

```
mkdir -p ~/workspaces/isaac_ros-dev/src && cd ~/workspaces/isaac_ros-dev/src
git clone git@github.com:NamTruongTran/Appras_Bearcar_24.git
```

Add these lines to your bashrc:

  - This is a alias for launching the container  
```
alias go="cd ~/workspaces/isaac_ros-dev/src/isaac_ros_common && ./scripts/run_dev.sh ~/workspaces/isaac_ros-dev"
```
  - This command sets an environment variable
```
export ISAAC_ROS_WS=~/workspaces/isaac_ros-dev/
```

### Isaac ROS Docker Environment

This project based on the Isaac ROS Dev Docker image, which contains predefined dependencies and settings. 
On top of the base image, we add our own layer in order to add additional packages.

The following table shows different alias for the environment:


| Alias                              | Programm Call | Description  |
|----------                          |----------     |----------    |
| go                                 | cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \ ./scripts/run_dev.sh            | Launch the Isaac ROS Docker Container (specified in the .bashrc outside of the container)                                           |
| sr                                 | source /workspaces/isaac_ros-dev/install/setup.bash                          | Source the workspace (the workspace will be sourced automatically after initially starting the container or attaching a new one)    |
| cb                                 | colcon build --symlink-install                                               | Build the workspace                                                                                                                 |  
| vslam_go                           | ros2 launch isaac_ros_visual_slam isaac_ros_visual_slam_realsense.launch.py  | Start the Isaac ROS Visual SLAM Node with Realsense node and the corresponding frames for Bearcar                                   |
| keypoints                          | ros2 run keypoints_visualizer vslam_sim                                      | Show projected 3D points of the Pointcloud2 Topic onto the 2D image plane                                                           |

## Experimental Accuracy Results

| Experiment                      | 1st Run    | 2nd Run    | 3rd Run    | Median Error |
|---------------------------------|------------|------------|------------|--------------|
| Whitewall                       | 46.007 %   | 41.639 %   | 50.180 %   | 34.456 %     |
| Short Distance (slow)           | 2.485 %    | 1.671 %    | 1.945 %    | 2.034 %      |
| Short Distance (fast)           | 2.606 %    | 2.848 %    | 2.111 %    | 2.521 %      |
| Long Distance (slow)            | 2.501 %    | 2.477 %    | 1.603 %    | 2.193 %      |
| Long Distance (fast)            | 2.687 %    | 2.806 %    | 2.260 %    | 2.338 %      |
| Rotation                        | 0.050 %    | 0.372 %    | 3.266 %    | 1.229 %      |
| Loop Closure (translation)      | 5.995 cm   | 4.338 cm   | 6.444 cm   | 5.592 cm     |
| Loop Closure (rotation)         | 0.077 %    | 0.105 %    | 0.583 %    | 0.255 %      |

## Additional Documentation:
- First steps to set up ISAAC ROS on the Jetson Xavier AGX
     - [Get-Started](https://web.archive.org/web/20240226200225/https://nvidia-isaac-ros.github.io/getting_started/index.html)
- Nvidia ISAAC ROS Nitros for acceleration of ROS nodes with type adaption and negotiation
     - [ISAAC-ROS-Nitros](https://web.archive.org/web/20240413190927/https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html)
 - Nvidia Isaac ROS Visual SLAM Package enables odometry estimation with Visual Odometry 
     - [ISAAC-ROS-Visual-SLAM](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_visual_slam/isaac_ros_visual_slam/index.html#quickstart)
- Nvidia Isaac ROS Visual SLAM with Realsense camera
     - [ISAAC-ROS-Visual-SLAM-Realsense](https://web.archive.org/web/20240226195409/https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_realsense.html)

