cmake_minimum_required(VERSION 2.8.3)
project(serializable)

find_package(catkin REQUIRED COMPONENTS roscpp rospy)

catkin_package(
   INCLUDE_DIRS include
   LIBRARIES ${PROJECT_NAME}
   CATKIN_DEPENDS roscpp rospy
)

include_directories(include
  ${catkin_INCLUDE_DIRS}
)


#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)


#common commands for building c++ executables and libraries
add_library(${PROJECT_NAME} src/lib/serializable.cpp)

catkin_add_gtest(test_serializable src/test/test_serializable.cpp)
target_link_libraries(test_serializable ${PROJECT_NAME})
