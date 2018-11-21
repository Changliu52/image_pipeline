cmake_minimum_required(VERSION 2.8.3)
project(image_proc_tegra)
set (CMAKE_CXX_STANDARD 11)

find_package(catkin REQUIRED COMPONENTS
  image_transport
  image_geometry
  sensor_msgs
  std_msgs
  roscpp
  nodelet  
)

set(cv_bridge_DIR "/home/nvidia/ws_opencv/deve/share/cv_bridge/cmake")
find_package(cv_bridge REQUIRED NO_CMAKE_PATH)

set(OpenCV_DIR "/home/nvidia/opencv/build")
#set(_include_dirs "include;/home/nvidia/opencv")
find_package(OpenCV 3.3.0 REQUIRED NO_CMAKE_PATH)


catkin_package(
  CATKIN_DEPENDS image_geometry roscpp sensor_msgs
  DEPENDS OpenCV
  LIBRARIES ${PROJECT_NAME}
)

include_directories(SYSTEM ${OpenCV_INCLUDE_DIRS} ${cv_bridge_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})
include_directories(include)

add_library(${PROJECT_NAME}     src/nodelets/rectify.cpp )
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES} ${cv_bridge_LIBRARIES} ${catkin_LIBRARIES})

add_executable(image_proc_tegra_exe src/image_proc_tegra.cpp)

target_link_libraries(image_proc_tegra_exe
   ${PROJECT_NAME}
   ${OpenCV_LIBRARIES} ${cv_bridge_LIBRARIES}
   ${catkin_LIBRARIES}
)
