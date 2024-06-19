cmake_minimum_required(VERSION 3.5)
project(sm_eigen)

set(CMAKE_CXX_STANDARD 14)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(Boost REQUIRED COMPONENTS system serialization)
find_package(Eigen3 REQUIRED)
find_package(sm_common REQUIRED)
find_package(sm_random REQUIRED)

set(CMAKE_CXX_FLAGS " ${CMAKE_CXX_FLAGS_INIT} -fPIC")

set(DEPENDENCIES
  Boost
  Eigen3
  sm_common
  sm_random
)

include_directories(include ${EIGEN3_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})

##############
## Building ##
##############

add_library(${PROJECT_NAME}  
  src/random.cpp
)

target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} sm_random::sm_random)

target_include_directories(${PROJECT_NAME} PUBLIC
  "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
  "$<INSTALL_INTERFACE:include>"
)

ament_target_dependencies(${PROJECT_NAME} ${DEPENDENCIES})

##################
## Installation ##
##################

## Mark cpp header files for installation
install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)

install(DIRECTORY include/
  DESTINATION include/ #include/${PROJECT_NAME}
)

#############
## Testing ##
#############
if(BUILD_TESTING)
  # Avoid clash with tr1::tuple: https://code.google.com/p/googletest/source/browse/trunk/README?r=589#257
  add_definitions(-DGTEST_USE_OWN_TR1_TUPLE=0)
  find_package(ament_cmake_gtest REQUIRED)

  ## Add gtest based cpp test target and link libraries
  ament_add_gtest(${PROJECT_NAME}-test
    test/test_main.cpp
    test/EigenSerializationTests.cpp
    test/MatrixSqrtTest.cpp
  )

  target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
  install(TARGETS 
    ${PROJECT_NAME}-test
    DESTINATION lib/${PROJECT_NAME}
  )
endif()

ament_package()