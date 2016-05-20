# roomba_500_series
Refreshed project to be built within Catkin workspace with package.xml and CMakeLists.txt updated.
Tested for compilation in ROS Indigo

This assembly contains Roomba 400 interface from https://github.com/igor-nap/ros-roomba but these targets are not included into compile targets.
You may want to append them by copying and modifying CMakeLists.txt in the end.

Warning: this project depends on [cereal_port](https://github.com/NetBUG/cereal_port) which is by default not supported in Catkin.

P.S. Do not forget to add current user to _dialout_ group to allow access to ttyUSB device!