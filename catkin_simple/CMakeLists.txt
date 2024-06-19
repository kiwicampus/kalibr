cmake_minimum_required(VERSION 3.5)
project(catkin_simple)

find_package(ament_cmake REQUIRED)

catkin_package(
  CATKIN_DEPENDS catkin
  CFG_EXTRAS catkin_simple-extras.cmake
)
