cmake_minimum_required(VERSION 3.5)
project(sm_random)

find_package(ament_cmake REQUIRED)

include_directories(include)

##############
## Building ##
##############

add_library(${PROJECT_NAME}
  src/random.cpp
)

target_include_directories(${PROJECT_NAME} PUBLIC
  "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
  "$<INSTALL_INTERFACE:include>"
)

ament_export_targets(export_${PROJECT_NAME} HAS_LIBRARY_TARGET)

##################
## Installation ##
##################

install(TARGETS ${PROJECT_NAME}
  EXPORT export_${PROJECT_NAME} 
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

## Mark cpp header files for installation
install(DIRECTORY include/
  DESTINATION include
)

ament_package()