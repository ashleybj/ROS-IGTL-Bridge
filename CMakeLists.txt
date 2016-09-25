cmake_minimum_required(VERSION 2.8.3)
project(ros_igtl_bridge)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  sensor_msgs
  message_generation
  message_runtime
  roscpp
  rospy
  std_msgs
  cv_bridge

)

## System dependencies are found with CMake's conventions

find_package(VTK REQUIRED)
find_package(Boost REQUIRED COMPONENTS system thread)

## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()

################################################
## Declare ROS messages, services and actions ##
################################################

## To declare and build messages, services or actions from within this
## package, follow these steps:
## * Let MSG_DEP_SET be the set of packages whose message types you use in
##   your messages/services/actions (e.g. std_msgs, actionlib_msgs, ...).
## * In the file package.xml:
##   * add a build_depend tag for "message_generation"
##   * add a build_depend and a run_depend tag for each package in MSG_DEP_SET
##   * If MSG_DEP_SET isn't empty the following dependency has been pulled in
##     but can be declared for certainty nonetheless:
##     * add a run_depend tag for "message_runtime"
## * In this file (CMakeLists.txt):
##   * add "message_generation" and every package in MSG_DEP_SET to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * add "message_runtime" and every package in MSG_DEP_SET to
##     catkin_package(CATKIN_DEPENDS ...)
##   * uncomment the add_*_files sections below as needed
##     and list every .msg/.srv/.action file to be processed
##   * uncomment the generate_messages entry below
##   * add every package in MSG_DEP_SET to generate_messages(DEPENDENCIES ...)

## Generate messages in the 'msg' folder
 add_message_files(
   FILES
   igtltransform.msg
   igtlpoint.msg 
   igtlpointcloud.msg 
   igtlimage.msg
   igtlpolydata.msg
   igtlstring.msg
   vector.msg
 )

## Generate services in the 'srv' folder
# add_service_files(
#   FILES
#   Service1.srv
#   Service2.srv
# )

## Generate actions in the 'action' folder
# add_action_files(
#   FILES
#   Action1.action
#   Action2.action
# )

## Generate added messages and services with any dependencies listed here
 generate_messages(
  DEPENDENCIES
  geometry_msgs
  sensor_msgs
  std_msgs
  # Or other packages containing msgs
 )

################################################
## Declare ROS dynamic reconfigure parameters ##
################################################

## To declare and build dynamic reconfigure parameters within this
## package, follow these steps:
## * In the file package.xml:
##   * add a build_depend and a run_depend tag for "dynamic_reconfigure"
## * In this file (CMakeLists.txt):
##   * add "dynamic_reconfigure" to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * uncomment the "generate_dynamic_reconfigure_options" section below
##     and list every .cfg file to be processed

## Generate dynamic reconfigure parameters in the 'cfg' folder
# generate_dynamic_reconfigure_options(
#   cfg/DynReconf1.cfg
#   cfg/DynReconf2.cfg
# )

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  INCLUDE_DIRS  include ${CMAKE_CURRENT_LIST_DIR}/OpenIGTLink/include ${VTK_INCLUDE_DIRS}   ${catkin_INCLUDE_DIRS}
  CATKIN_DEPENDS message_runtime
 # LIBRARIES lib
#  CATKIN_DEPENDS other_catkin_pkg
#  DEPENDS libs
)


###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
 include_directories(include ${CMAKE_CURRENT_LIST_DIR}/OpenIGTLink/include ${CMAKE_CURRENT_LIST_DIR}/testing/include ${VTK_INCLUDE_DIRS})

## Declare a C++ library
 add_library(ROS_IGTL_Bridge_Lib
   	src/ROS_IGTL_Bridge.cpp
	include/ROS_IGTL_Bridge.h

 )

## Add cmake target dependencies of the library
## as an example, code may need to be generated before libraries
## either from message generation or dynamic reconfigure
# add_dependencies(ros_igtl_bridge ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Declare a C++ executable
# add_executable(ros_igtl_bridge_node src/ros_igtl_bridge_node.cpp)
add_executable(ROS_IGTL_Bridge src/ROS_IGTL_Bridge.cpp src/main.cpp)
add_executable(ROS_IGTL_Test testing/ROS_IGTL_Test.cpp testing/main_test.cpp)

## Add cmake target dependencies of the executable
## same as for the library above
# add_dependencies(ros_igtl_bridge_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
target_link_libraries(ROS_IGTL_Bridge ${catkin_LIBRARIES}  )
target_link_libraries(ROS_IGTL_Bridge ${VTK_LIBRARIES})
target_link_libraries(ROS_IGTL_Bridge ${CMAKE_CURRENT_LIST_DIR}/OpenIGTLink/libOpenIGTLink.a)
target_link_libraries(ROS_IGTL_Bridge ${CMAKE_CURRENT_LIST_DIR}/OpenIGTLink/libigtlutil.a)

target_link_libraries(ROS_IGTL_Bridge_Lib ${catkin_LIBRARIES})
target_link_libraries(ROS_IGTL_Bridge_Lib ${VTK_LIBRARIES})
target_link_libraries(ROS_IGTL_Bridge_Lib ${CMAKE_CURRENT_LIST_DIR}/OpenIGTLink/libOpenIGTLink.a)
target_link_libraries(ROS_IGTL_Bridge_Lib ${CMAKE_CURRENT_LIST_DIR}/OpenIGTLink/libigtlutil.a)

target_link_libraries(ROS_IGTL_Test ${catkin_LIBRARIES}  )
target_link_libraries(ROS_IGTL_Test ${VTK_LIBRARIES})
target_link_libraries(ROS_IGTL_Test ROS_IGTL_Bridge_Lib)

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS ros_igtl_bridge ros_igtl_bridge_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_ros_igtl_bridge.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)