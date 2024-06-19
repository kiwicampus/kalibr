cmake_minimum_required(VERSION 3.5)
project(python_module)

find_package(ament_cmake REQUIRED)

catkin_package(
  CFG_EXTRAS add_python_export_library.cmake
)