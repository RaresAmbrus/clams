cmake_minimum_required(VERSION 2.8.3)
project(clams)

find_package(catkin REQUIRED COMPONENTS roscpp rospy stream_sequence bag_of_tricks eigen_extensions timer)
find_package(Boost REQUIRED COMPONENTS program_options)
find_package(OpenCV REQUIRED)
find_package(PCL 1.7 REQUIRED)
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

find_package(VTK REQUIRED COMPONENTS vtkCommon vtkFiltering vtkRendering)
if(VTK_FOUND)
  include_directories(${VTK_INCLUDE_DIRS})
endif()

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS roscpp rospy 
  DEPENDS Boost stream_sequence bag_of_tricks 
)

include_directories(include
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
)

#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#common commands for building c++ executables and libraries
add_library(clams
  src/lib/discrete_depth_distortion_model.cpp
  src/lib/discrete_depth_distortion_model_helpers.cpp
  src/lib/slam_calibrator.cpp
  src/lib/trajectory_visualizer.cpp
  src/lib/trajectory.cpp
  )

set_target_properties(clams PROPERTIES COMPILE_FLAGS -fopenmp)
set_target_properties(clams PROPERTIES LINK_FLAGS -fopenmp)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${OpenCV_LIBS} bag_of_tricks stream_sequence)

add_executable(inspect src/prog/inspect.cpp)
target_link_libraries(inspect ${PROJECT_NAME} ${VTK_LIBRARIES})

add_executable(calibrate src/prog/calibrate.cpp)
target_link_libraries(calibrate ${PROJECT_NAME})

add_executable(visualize_model src/prog/visualize_model.cpp)
target_link_libraries(visualize_model ${PROJECT_NAME})

add_executable(convert_trajectory src/prog/convert_trajectory.cpp)
target_link_libraries(convert_trajectory ${PROJECT_NAME})

add_executable(visualize_trajectory src/prog/visualize_trajectory.cpp)
target_link_libraries(visualize_trajectory ${PROJECT_NAME})

add_executable(generate_map src/prog/generate_map.cpp)
target_link_libraries(generate_map ${PROJECT_NAME})
