cmake_minimum_required(VERSION 2.8)
project(depth_fisheye_image_proc)

find_package(catkin REQUIRED cmake_modules eigen_conversions image_geometry image_transport message_filters nodelet sensor_msgs stereo_msgs tf2 tf2_ros)

if(cv_bridge_VERSION VERSION_GREATER "1.12.0")
  add_compile_options(-std=c++11)
endif()

catkin_package(
    INCLUDE_DIRS include
    LIBRARIES ${PROJECT_NAME})

find_package(Boost REQUIRED)
find_package(Eigen3 QUIET)
if(NOT EIGEN3_FOUND)
  find_package(Eigen REQUIRED)
  set(EIGEN3_INCLUDE_DIRS ${EIGEN_INCLUDE_DIRS})
endif()

set(cv_bridge_DIR "/home/nvidia/ws_opencv/deve/share/cv_bridge/cmake")
find_package(cv_bridge REQUIRED NO_CMAKE_PATH)

set(OpenCV_DIR "/home/nvidia/opencv/build")
#set(_include_dirs "include;/home/nvidia/opencv")
find_package(OpenCV 3 REQUIRED NO_CMAKE_PATH)


include_directories(include ${OpenCV_INCLUDE_DIRS} ${cv_bridge_INCLUDE_DIRS} ${BOOST_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIRS})

add_library(${PROJECT_NAME} 
                             src/nodelets/point_cloud_xyz.cpp
                             #src/nodelets/point_cloud_xyzrgb.cpp


)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS} ${cv_bridge_LIBRARIES} ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})


add_executable(depth_fisheye_image_proc_exe src/depth_fisheye_image_proc.cpp)


target_link_libraries(depth_fisheye_image_proc_exe
   ${PROJECT_NAME}
   ${catkin_LIBRARIES}
)


install(DIRECTORY include/${PROJECT_NAME}/
    DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
    FILES_MATCHING PATTERN "*.h")

install(TARGETS ${PROJECT_NAME}
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)
install(FILES nodelet_plugins.xml
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)










