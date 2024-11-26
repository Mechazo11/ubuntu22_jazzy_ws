[![License: BSD-3-Clause](https://img.shields.io/badge/License-BSD--3--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)
[![ROS 2 Jazzy](https://img.shields.io/badge/ROS--2-Jazzy-blue)](https://docs.ros.org/en/jazzy/index.html)
[![Gazebo Harmonic](https://img.shields.io/badge/Gazebo-Harmonic-orange)](https://gazebosim.org/)

# Ubuntu 22.04 ROS 2 Jazzy Workspace

A modified source build of ROS 2 Jazzy Workspace compatible with Ubuntu 22.04. It comes equipped with packages for controlling robots with ```ros2_control``` and all pertinent packages to interface with ```Gazebo Harmonic```, ```Movit2``` and ```Nav2```.

## Features

* Uses **CycloneDDS** middleware
* Added **ros2_control**, **ros2_controllers** and all **vendor** packages to interface with Gazebo Harmonic
* Added **vision_opencv** packages with corrected Cmake for compatibility with Boost 1.74 and Python 3.10

### Prerequisite note

* Before you build this workspace please ensure no ROS 2 Humble workspaces be it the global workspace ```source /opt/ros/humble/setup.bash``` or any of its variant is sourced. This will cause a build failure most notably with Jazzy version of ```ros2_control```. Check this [issue](https://github.com/ros-controls/ros2_control/issues/1787) for more details.
* Building from source may take 20 - 30 minutes to complete.
* Please allot about 8GB of space, 16GB (32 GB recommended) RAM

## Build

```bash
cd ~
git clone https://github.com/Mechazo11/ubuntu22_jazzy_ws.git
cd ubuntu22_jazzy_ws/
sudo rosdep init
rosdep update
vcs import src < jazzy.repos --recursive

rosdep install -r --from-paths src --ignore-src -y --rosdistro jazzy --skip-keys "fastrtps
fastcdr rmw_fastrtps_cpp rmw_fastrtps_dynamic_cpp rmw_fastrtps_shared_cpp rmw_connextdds rosidl_typesupport_fastrtps_c rosidl_typesupport_fastrtps_cpp fastrtps_cmake_module demo_nodes_py rti-connext-dds-6.0.1 urdfdom_headers"

colcon build --packages-up-to ament_cmake_ros
source ./install/setup.bash
colcon build --cmake-args -DCMAKE_CXX_FLAGS="-w"
```