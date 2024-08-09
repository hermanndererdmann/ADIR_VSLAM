
# Visual SLAM Bachelor Thesis

<div style="display: flex; align-items: center; justify-content: center;">
    <a href="https://www.antrobotics.de" target="_blank" style="margin-right: 20px;">
        <img src="https://www.antrobotics.de/wp-content/uploads/2024/07/cropped-Ant-Robotics.png" alt="Ant Robotics Logo" width="250">
    </a>
    <a href="https://www.fhgr.ch" target="_blank">
        <img src="https://www.fhgr.ch/_assets/feb1d86054ab2e718cccf9db6ed46074/Partials/Logo/Images/Logo.svg" alt="FHGR Logo" width="250">
    </a>
</div>

## Features

- Visual/Visual-Inertial SLAM implementation
- Can run with one or multiple[^1] Stereo Cams
- GPU based
- NVIDIA Jetson & ZED 2i
- ROS2
- Created for ANT-Robotic Adir platform



## Requirements
- NVIDIA GPU with CUDA
- [ZED-SDK](https://www.stereolabs.com/en-ch/developers/release) installed
- Camera with at least 30FPS

## Installation

At the moment of the release the SLAM package does not support building the it from source for the ZED camera since it's currently not supported.
```
mkdir -p ~/ros2_ws/src/ # create your workspace if it does not exist
git clone  --recursive https://github.com/hermanndererdmann/ADIR_VSLAM.git
cd ..
sudo apt update
rosdep update
rosdep install --from-paths src --ignore-src -r -y # install dependencies
colcon build --symlink-install --cmake-args=-DCMAKE_BUILD_TYPE=Release --parallel-workers $(nproc) # build the workspace
source install/setup.bash
```

[This guide](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_visual_slam/issues/164#issuecomment-2168734449) can be help to get it working for the ISAAC ROS 3.0.1 Version.



## Launch
Launch each in a seperate terminal with sources workspace.
```
ros2 launch zed_wrapper zed_camera.launch.py camera_model:='zed2i'
```
```
ros2 launch adir_amr_description display.launch.py
```
```
rviz2 -d src/nvidia_vslam/rviz/default.cfg.rviz
```
For VI-SLAM
```
ros2 launch nvidia_vslam visual-inertial.launch.py
```
or V-SLAM
```
ros2 launch nvidia_vslam visual.launch.py
```




## Things to keep in mind
For visual-inertial SLAM the Allan variance of the IMU has to be calculated. This can be done with [this](https://github.com/Autoliv-Research/allan_variance_ros2) repo (readme not up to date).
It is recommended to record 15-24h of IMU topics to a ROSBAG. The IMU has to be perfectly still during that time.

These calculated values have to be put in the NVIDIA VSLAM launch files.

## Links

[Official NVIDIA cuVSLAM Documentation](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html)



## Changelog

#### Version 1.0.0 
Released 09/08/2024:
- Inital release (end of thesis)


[^1]: Not implemented/tested for ZED yet.
