# Ubuntu 22.04 ROS 2 Jazzy Workspace

A modified source build of ROS 2 Jazzy Workspace compatible with Ubuntu 22.04. 

## Modifications

* Compiled to only support **CycloneDDS** middleware
* Removed all packages related to gazebo, those will be made available on a separate workspace
* Added ros2_control

## Build

```bash
cd ~
git clone https://github.com/Mechazo11/ubuntu22_jazzy_ws.git
cd ubuntu22_jazzy_ws/
sudo rosdep init
rosdep update
vcs import src < jazzy.repos

rosdep install --from-paths src --ignore-src -y --skip-keys "fastrtps
fastcdr rmw_fastrtps_cpp rmw_fastrtps_dynamic_cpp rmw_fastrtps_shared_cpp rmw_connextdds rosidl_typesupport_fastrtps_c rosidl_typesupport_fastrtps_cpp fastrtps_cmake_module demo_nodes_py rti-connext-dds-6.0.1 urdfdom_headers"

colcon build --symlink-install --cmake-args -DCMAKE_CXX_FLAGS="-w"
```
