cmake_minimum_required(VERSION 3.5)
project(sm_opencv)

set(CMAKE_CXX_STANDARD 14)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sm_common REQUIRED)
find_package(Boost REQUIRED COMPONENTS system)

catkin_package(
  INCLUDE_DIRS include  ${catkin_INCLUDE_DIRS}
  CATKIN_DEPENDS sm_common
  DEPENDS Boost
)
add_definitions(-D__STRICT_ANSI__)

install(DIRECTORY include/
  DESTINATION include
)
