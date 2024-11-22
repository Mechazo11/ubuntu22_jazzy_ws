# Ubuntu 22.04 ROS 2 Jazzy Workspace

A modified source build of ROS 2 Jazzy Workspace compatible with Ubuntu 22.04. 

## Modifications

* By default uses **CycloneDDS** middleware
* Added **ros2_control**, **ros2_controllers** and its relevant packages.
* Added **vision_opencv** packages with corrected Cmake for compatibility with Boost 1.74 and Python 3.10

### Prerequisite note

* Before you build this workspace please ensure no ROS 2 Humble workspaces be it the global workspace ```source /opt/ros/humble/setup.bash``` or any of its variant is sourced. This will cause a build failure most notably with Jazzy version of ```ros2_control```. Check this [issue](https://github.com/ros-controls/ros2_control/issues/1787) for more details.
* Building from source may take 20 - 30 minutes to complete.
* Please allot about 8GB of space, 16GB (32 GB recommended) RAM
* 

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


* Need to fix this

```bash

--- stderr: mecanum_drive_controller                                                        
/home/tigerwife/ubuntu22_jazzy_ws/src/ros-controls/ros2_controllers/mecanum_drive_controller/src/mecanum_drive_controller.cpp: In member function ‘virtual controller_interface::CallbackReturn mecanum_drive_controller::MecanumDriveController::on_deactivate(const rclcpp_lifecycle::State&)’:
/home/tigerwife/ubuntu22_jazzy_ws/src/ros-controls/ros2_controllers/mecanum_drive_controller/src/mecanum_drive_controller.cpp:316:39: error: void value not ignored as it ought to be
  316 |       command_interfaces_[i].set_value(std::numeric_limits<double>::quiet_NaN());
      |       ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/home/tigerwife/ubuntu22_jazzy_ws/src/ros-controls/ros2_controllers/mecanum_drive_controller/src/mecanum_drive_controller.cpp: In member function ‘virtual controller_interface::return_type mecanum_drive_controller::MecanumDriveController::update_and_write_commands(const rclcpp::Time&, const rclcpp::Duration&)’:
/home/tigerwife/ubuntu22_jazzy_ws/src/ros-controls/ros2_controllers/mecanum_drive_controller/src/mecanum_drive_controller.cpp:447:48: error: could not convert ‘(&((mecanum_drive_controller::MecanumDriveController*)this)->mecanum_drive_controller::MecanumDriveController::<anonymous>.controller_interface::ChainableControllerInterface::<anonymous>.controller_interface::ControllerInterfaceBase::command_interfaces_.std::vector<hardware_interface::LoanedCommandInterface>::operator[](((std::vector<hardware_interface::LoanedCommandInterface>::size_type)mecanum_drive_controller::MecanumDriveController::FRONT_LEFT)))->hardware_interface::LoanedCommandInterface::set_value(((double)wheel_front_left_vel))’ from ‘void’ to ‘bool’
  447 |       command_interfaces_[FRONT_LEFT].set_value(wheel_front_left_vel) &&
      |       ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~
/home/tigerwife/ubuntu22_jazzy_ws/src/ros-controls/ros2_controllers/mecanum_drive_controller/src/mecanum_drive_controller.cpp:448:49: error: could not convert ‘(&((mecanum_drive_controller::MecanumDriveController*)this)->mecanum_drive_controller::MecanumDriveController::<anonymous>.controller_interface::ChainableControllerInterface::<anonymous>.controller_interface::ControllerInterfaceBase::command_interfaces_.std::vector<hardware_interface::LoanedCommandInterface>::operator[](((std::vector<hardware_interface::LoanedCommandInterface>::size_type)mecanum_drive_controller::MecanumDriveController::FRONT_RIGHT)))->hardware_interface::LoanedCommandInterface::set_value(((double)wheel_front_right_vel))’ from ‘void’ to ‘bool’
  448 |       command_interfaces_[FRONT_RIGHT].set_value(wheel_front_right_vel) &&
      |       ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~
/home/tigerwife/ubuntu22_jazzy_ws/src/ros-controls/ros2_controllers/mecanum_drive_controller/src/mecanum_drive_controller.cpp:458:75: error: could not convert ‘(&((mecanum_drive_controller::MecanumDriveController*)this)->mecanum_drive_controller::MecanumDriveController::<anonymous>.controller_interface::ChainableControllerInterface::<anonymous>.controller_interface::ControllerInterfaceBase::command_interfaces_.std::vector<hardware_interface::LoanedCommandInterface>::operator[](((std::vector<hardware_interface::LoanedCommandInterface>::size_type)mecanum_drive_controller::MecanumDriveController::FRONT_LEFT)))->hardware_interface::LoanedCommandInterface::set_value(0.0)’ from ‘void’ to ‘bool’
  458 |     const bool value_set_error = command_interfaces_[FRONT_LEFT].set_value(0.0) &&
      |                                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~
/home/tigerwife/ubuntu22_jazzy_ws/src/ros-controls/ros2_controllers/mecanum_drive_controller/src/mecanum_drive_controller.cpp:459:76: error: could not convert ‘(&((mecanum_drive_controller::MecanumDriveController*)this)->mecanum_drive_controller::MecanumDriveController::<anonymous>.controller_interface::ChainableControllerInterface::<anonymous>.controller_interface::ControllerInterfaceBase::command_interfaces_.std::vector<hardware_interface::LoanedCommandInterface>::operator[](((std::vector<hardware_interface::LoanedCommandInterface>::size_type)mecanum_drive_controller::MecanumDriveController::FRONT_RIGHT)))->hardware_interface::LoanedCommandInterface::set_value(0.0)’ from ‘void’ to ‘bool’
  459 |                                  command_interfaces_[FRONT_RIGHT].set_value(0.0) &&
      |                                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~
gmake[2]: *** [CMakeFiles/mecanum_drive_controller.dir/build.make:76: CMakeFiles/mecanum_drive_controller.dir/src/mecanum_drive_controller.cpp.o] Error 1
gmake[1]: *** [CMakeFiles/Makefile2:208: CMakeFiles/mecanum_drive_controller.dir/all] Error 2
gmake: *** [Makefile:146: all] Error 2
---

```