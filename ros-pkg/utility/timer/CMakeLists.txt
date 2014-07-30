cmake_minimum_required(VERSION 2.8.3)
project(timer)

find_package(catkin REQUIRED COMPONENTS roscpp rospy)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS roscpp rospy 
  DEPENDS rt
)

include_directories(include
  ${catkin_INCLUDE_DIRS}
)

#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

add_library(${PROJECT_NAME}
  src/lib/timer.cpp
  )

target_link_libraries(${PROJECT_NAME} rt)

catkin_add_gtest(test_timer src/test/test_timer.cpp)
#rosbuild_add_gtest_build_flags(test_timer)
target_link_libraries(test_timer ${PROJECT_NAME})




