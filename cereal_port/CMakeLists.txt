cmake_minimum_required(VERSION 2.8.3)
project(cereal_port)

catkin_package(
   INCLUDE_DIRS include
   LIBRARIES cereal_port
)

include_directories(include)

add_library(cereal_port src/cereal_port.cpp)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE)