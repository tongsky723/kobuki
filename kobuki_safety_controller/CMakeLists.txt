cmake_minimum_required(VERSION 2.8.3)
project(kobuki_safety_controller)
find_package(catkin REQUIRED COMPONENTS rospy roscpp nodelet pluginlib std_msgs geometry_msgs kobuki_msgs yocs_controllers ecl_threads dynamic_reconfigure)

generate_dynamic_reconfigure_options(
  cfg/Reaction.cfg
)

catkin_package(
   INCLUDE_DIRS include
   LIBRARIES kobuki_safety_controller_nodelet
   CATKIN_DEPENDS rospy roscpp nodelet pluginlib std_msgs geometry_msgs kobuki_msgs yocs_controllers ecl_threads dynamic_reconfigure
)


include_directories(include
                    ${catkin_INCLUDE_DIRS})

include_directories(${dynamic_reconfigure_PACKAGE_PATH}/cmake/cfgbuild.cmake)
add_library(kobuki_safety_controller_nodelet src/nodelet.cpp)
add_dependencies(kobuki_safety_controller_nodelet kobuki_msgs_gencpp)
add_dependencies(kobuki_safety_controller_nodelet ${PROJECT_NAME}_gencfg)
target_link_libraries(kobuki_safety_controller_nodelet ${catkin_LIBRARIES})

install(TARGETS kobuki_safety_controller_nodelet
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)
install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)
install(DIRECTORY plugins
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
install(DIRECTORY launch
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
