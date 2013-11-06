cmake_minimum_required(VERSION 2.8.3)
project(pluginlib)

find_package(catkin REQUIRED COMPONENTS class_loader rosconsole roslib)
find_package(Boost REQUIRED COMPONENTS filesystem system)

catkin_python_setup()

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES
  CATKIN_DEPENDS class_loader rosconsole roslib
  DEPENDS Boost
)

include_directories(include ${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})

install(DIRECTORY include/pluginlib/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})

install(FILES share/pluginlib/typed_class_loader_template.cpp
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

if(CATKIN_ENABLE_TESTING)
  add_library(test_plugins EXCLUDE_FROM_ALL SHARED test/test_plugins.cpp)

  catkin_add_gtest(${PROJECT_NAME}_utest test/utest.cpp)
  target_link_libraries(${PROJECT_NAME}_utest ${catkin_LIBRARIES} ${Boost_LIBRARIES})
  add_dependencies(${PROJECT_NAME}_utest test_plugins)
endif()
