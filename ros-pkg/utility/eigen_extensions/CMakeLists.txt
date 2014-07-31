cmake_minimum_required(VERSION 2.8.3)
project(eigen_extensions)

find_package(catkin REQUIRED COMPONENTS roscpp rospy gzstream timer)
find_package(Boost REQUIRED COMPONENTS system filesystem)
find_package(Eigen REQUIRED COMPONENTS system filesystem)

#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS roscpp rospy
  DEPENDS Boost Eigen gzstream 
)

include_directories(include
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
)

add_library(${PROJECT_NAME}
  src/random.cpp
  )

target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} gzstream timer)

catkin_add_gtest(test_random src/test_random.cpp)
target_link_libraries(test_random ${PROJECT_NAME}) 

catkin_add_gtest(test_eigen_extensions src/test_eigen_extensions.cpp)
target_link_libraries(test_eigen_extensions ${Boost_LIBRARIES} gzstream)

add_executable(cat src/cat.cpp)
target_link_libraries(cat ${Boost_LIBRARIES} gzstream)

add_executable(convert src/convert.cpp)
target_link_libraries(convert ${Boost_LIBRARIES} gzstream)


