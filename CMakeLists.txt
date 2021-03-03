cmake_minimum_required(VERSION 3.5)
project(roboclaw_ros2)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

find_package(ament_cmake REQUIRED)

find_package(rclcpp REQUIRED)
find_package(rclcpp_components REQUIRED)

#find_package(PkgConfig)
#pkg_check_modules(PC_OPENNI2 libopenni2)
#if (NOT PC_OPENNI2_FOUND)
#  pkg_check_modules(PC_OPENNI2 REQUIRED openni2)
#endif()

include_directories(
  include
)

#link_directories(${PC_OPENNI2_LIBRARY_DIRS})

add_library(roboclaw_ros2_driver SHARED
  src/RoboClawFS.cpp
)
target_link_libraries(roboclaw_ros2_driver
)
ament_target_dependencies(roboclaw_ros2_driver
  sensor_msgs
  rclcpp
)

add_library(roboclaw_ros2_driver SHARED
  src/RoboClawFS.cpp
)
target_link_libraries(roboclaw_ros2_driver
  roboclaw_ros2_driver
)
ament_target_dependencies(roboclaw_ros2_driver
  sensor_msgs
  rclcpp
)

rclcpp_components_register_node(roboclaw_ros2_driver
  PLUGIN "openni2_wrapper::OpenNI2Driver"
  EXECUTABLE roboclaw_ros2_driver
)

add_executable(list_devices
   src/list_devices.cpp
)
target_link_libraries(list_devices
  roboclaw_ros2_driver
)

add_executable(openni2_param_subscriber test/openni2_subscriber.cpp)
target_link_libraries(openni2_param_subscriber)
ament_target_dependencies(openni2_param_subscriber
#  image_transport
#  openni2_camera_param_msgs
  sensor_msgs
  rclcpp
)

install(
  TARGETS
    list_devices
    openni2_param_wrapper
    openni2_camera_param
    openni2_param_subscriber
    ${ADDITIONAL_EXECUTABLES}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

install(
  DIRECTORY include/
  DESTINATION include
)

install(
  DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}/
)

ament_export_include_directories(include)
ament_export_libraries(openni2_param_wrapper)
ament_export_dependencies(
  sensor_msgs
  rclcpp
)

ament_package()

