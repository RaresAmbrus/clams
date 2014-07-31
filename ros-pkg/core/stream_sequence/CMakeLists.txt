cmake_minimum_required(VERSION 2.8.3)
project(stream_sequence)

find_package(catkin REQUIRED COMPONENTS roscpp rospy rosconsole serializable eigen_extensions timer)
find_package(Boost REQUIRED COMPONENTS system program_options)
find_package(OpenCV REQUIRED)
find_package(PCL 1.7 REQUIRED)

link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS roscpp rospy
  DEPENDS Boost roscpp rosconsole OpenCV serializable eigen_extensions
)

include_directories(include
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
)

add_library(stream_sequence
  src/lib/stream_sequence_base.cpp
  src/lib/stream_sequence_pcl_wrapper.cpp
  src/lib/stream_sequence.cpp
  src/lib/frame_projector.cpp
  )

target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${PCL_LIBRARIES} ${OpenCV_LIBS} ${rosconsole_LIBRARIES} serializable eigen_extensions)
set_target_properties(stream_sequence PROPERTIES COMPILE_FLAGS -Wno-deprecated -Wno-unused-but-set-variable -Wno-unknown-pragmas)

add_executable(sseq_to_freiburg src/prog/sseq_to_freiburg.cpp)
target_link_libraries(sseq_to_freiburg ${PROJECT_NAME}   )
