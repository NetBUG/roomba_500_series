cmake_minimum_required(VERSION 2.4.6)
project(roomba_500_series)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rosconsole
  sensor_msgs
  geometry_msgs
  actionlib_msgs
  cereal_port
  tf
)


# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

#include(${actionlib_msgs_PACKAGE_PATH}/cmake/actionbuild.cmake)
#genaction()

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS roscpp rosconsole geometry_msgs
  DEPENDS geometry_msgs
)

include_directories(
  include
  msg_gen/cpp/include
  ${catkin_INCLUDE_DIRS}
  ${CEREAL_PORT_INCLUDE_DIRS}
)

#~rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
add_library(irobot_oi src/iRobot/OpenInterface.cpp)
add_library(go_dock src/GoDock/GoDock.cpp)

add_executable(roomba560_node src/roomba560.cpp src/iRobot/OpenInterface.cpp src/GoDock/GoDock.cpp)
target_link_libraries(roomba560_node cereal_port ${catkin_LIBRARIES})

add_executable(roomba500_light_node src/roomba500_lightweight.cpp src/iRobot/OpenInterface.cpp)
target_link_libraries(roomba500_light_node cereal_port ${catkin_LIBRARIES})

add_executable(godock_server src/godock_server.cpp src/GoDock/GoDock.cpp)
target_link_libraries(godock_server ${catkin_LIBRARIES})

add_executable(godock_client src/godock_client.cpp)
target_link_libraries(godock_client ${catkin_LIBRARIES})

install(TARGETS roomba560_node roomba500_light_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

#install(DIRECTORY launch rviz sdk
#  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
#  USE_SOURCE_PERMISSIONS
#)