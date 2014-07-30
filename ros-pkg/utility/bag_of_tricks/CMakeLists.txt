cmake_minimum_required(VERSION 2.8.3)
project(bag_of_tricks)

find_package(catkin REQUIRED COMPONENTS roscpp rospy std_msgs serializable)
find_package(Boost REQUIRED COMPONENTS thread)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS roscpp rospy std_msgs
  DEPENDS Boost rt serializable
)

include_directories(include
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
)



set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

add_library(${PROJECT_NAME}
  src/high_res_timer.cpp
  src/lockable.cpp
  )

target_link_libraries(${PROJECT_NAME} rt ${Boost_LIBRARIES} serializable)

catkin_add_gtest(test_hrt src/test_hrt.cpp)
target_link_libraries(test_hrt ${PROJECT_NAME} rt ${Boost_LIBRARIES} serializable)

catkin_add_gtest(test_lockable src/test_lockable.cpp)
target_link_libraries(test_lockable ${PROJECT_NAME} rt ${Boost_LIBRARIES} serializable)



