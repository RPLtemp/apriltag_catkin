cmake_minimum_required(VERSION 2.8.3)
project(apriltag_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(apriltag_src
  SVN_REPOSITORY https://svn.csail.mit.edu/apriltags
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND ""
  BUILD_COMMAND cd ../apriltag_src && make BUILD_PREFIX=${CATKIN_DEVEL_PREFIX}
  INSTALL_COMMAND ""
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} apriltag_src)
target_link_libraries(${PROJECT_NAME} ${CATKIN_DEVEL_PREFIX}/lib/libapriltags${CMAKE_STATIC_LIBRARY_SUFFIX})

cs_install()

cs_export(
  INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS apriltag-extras.cmake
)
