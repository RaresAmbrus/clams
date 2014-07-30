cmake_minimum_required(VERSION 2.8.3)
project(gzstream)

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

find_package(catkin REQUIRED COMPONENTS roscpp rospy)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS roscpp rospy 
)

include_directories(include
  ${catkin_INCLUDE_DIRS}
)

#common commands for building c++ executables and libraries
add_library(${PROJECT_NAME} src/gzstream.cpp)
target_link_libraries(${PROJECT_NAME} z)

add_executable(test_gzip src/test_gzip.cpp)
target_link_libraries(test_gzip ${PROJECT_NAME})

add_executable(test_gunzip src/test_gunzip.cpp)
target_link_libraries(test_gunzip ${PROJECT_NAME})
