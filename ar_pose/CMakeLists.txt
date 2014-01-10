cmake_minimum_required(VERSION 2.8.3)
project(ar_pose)

# Load catkin and all dependencies required for this package
find_package(catkin REQUIRED COMPONENTS  
  libartoolkit 
  geometry_msgs 
  tf 
  resource_retriever 
  image_transport 
  cv_bridge
  message_generation)
find_package(OpenCV REQUIRED)
find_package(GLUT REQUIRED)

add_message_files( 
  DIRECTORY msg 
  FILES ARMarker.msg 
        ARMarkers.msg)

generate_messages(
  DEPENDENCIES geometry_msgs)

include_directories(include ${OpenCV_INCLUDE_DIRS} ${GLUT_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})

#include_directories('/usr/local/include/')
#link_directories('/usr/local/lib/')

add_executable(ar_single  
                src/ar_single.cpp)
target_link_libraries(ar_single 
  GLU GL glut ARgsub AR ARMulti ARvideo ${OpenCV_LIBRARIES} ${catkin_LIBRARIES})
add_executable(ar_multi  
                src/ar_multi.cpp
                src/object.cpp)
target_link_libraries(ar_multi
  GLU GL glut ARgsub AR ARMulti ARvideo ${OpenCV_LIBRARIES} ${catkin_LIBRARIES})

# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
catkin_package(
    DEPENDS OpenCV GLUT
    CATKIN_DEPENDS libartoolkit geometry_msgs tf resource_retriever image_transport cv_bridge message_runtime
    INCLUDE_DIRS include
)
