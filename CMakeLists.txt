cmake_minimum_required(VERSION 2.8.3)
project(turtlebot_rrt)
add_definitions(-std=c++11 -Wall -Werror)

find_package(catkin REQUIRED 
  COMPONENTS
    roscpp
    roslint
    tf
    nav_core
    costmap_2d
    base_local_planner
    pluginlib
    visualization_msgs
)

#find_package(Eigen3 REQUIRED)

include_directories(include ${catkin_INCLUDE_DIRS})

#add_definitions(${EIGEN3_DEFINITIONS})

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS
    roscpp
    pluginlib
    costmap_2d
    base_local_planner
    nav_core
    visualization_msgs
)

add_library(${PROJECT_NAME} 
  src/turtlebot_rrt.cc
  src/vertex.cc
)

add_dependencies(turtlebot_rrt ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})


# Testing
if (CATKIN_ENABLE_TESTING)
  catkin_add_gtest(vertex_test test/vertex_test.cc)
  catkin_add_gtest(rrt_test test/rrtpath_test.cc)

  target_link_libraries(rrt_test ${PROJECT_NAME} ${catkin_LIBRARIES})
  target_link_libraries(vertex_test ${catkin_LIBRARIES} ${PROJECT_NAME})
endif (CATKIN_ENABLE_TESTING)

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/ DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})
install(FILES rrt_planner_plugin.xml DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})  
