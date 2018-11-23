cmake_minimum_required(VERSION 2.8.3)
project(image_proc_fisheye)
set (CMAKE_CXX_STANDARD 11)

find_package(catkin REQUIRED COMPONENTS
  image_transport
  sensor_msgs
  std_msgs
  roscpp
  nodelet  
)


find_package(cv_bridge REQUIRED 
PATHS /home/nvidia/ws_opencv/devel 
NO_DEFAULT_PATH)

find_package(image_geometry REQUIRED 
PATHS /home/nvidia/ws_opencv/devel 
NO_DEFAULT_PATH)

set(OpenCV_DIR "/home/nvidia/opencv/build")
#set(_include_dirs "include;/home/nvidia/opencv")
find_package(OpenCV 3 REQUIRED NO_CMAKE_PATH)

catkin_package(
  CATKIN_DEPENDS image_geometry roscpp sensor_msgs
  DEPENDS OpenCV
  LIBRARIES ${PROJECT_NAME}
)

message(WARNING "OpenCV_VERSION: ${OpenCV_VERSION}")
message(WARNING "OpenCV_INCLUDE_DIRS: ${OpenCV_INCLUDE_DIRS}")
message(WARNING "OpenCV_LIBRARIES: ${OpenCV_LIBRARIES}")
message(WARNING "OpenCV_LIBS: ${OpenCV_LIBS}")
message(WARNING "CMAKE_PREFIX_PATH: ${CMAKE_PREFIX_PATH}")
message(WARNING "LD_LIBRARY_PATH: ${LD_LIBRARY_PATH}")
message(WARNING "_include_dirs: ${_include_dirs}")

include_directories(SYSTEM ${OpenCV_INCLUDE_DIRS} ${cv_bridge_INCLUDE_DIRS} ${image_geometry_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})
include_directories(include)

add_library(${PROJECT_NAME}     src/nodelets/rectify_.cpp )
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES} ${cv_bridge_LIBRARIES} ${image_geometry_LIBRARIES} ${catkin_LIBRARIES})

add_executable(image_proc_fisheye_exe src/image_proc_fisheye.cpp)

target_link_libraries(image_proc_fisheye_exe
   ${PROJECT_NAME}
   ${catkin_LIBRARIES}
)
