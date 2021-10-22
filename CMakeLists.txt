cmake_minimum_required(VERSION 3.0.2)
project(nlopt_ros)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED)
include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

# set(VERSION 2.4.1)
# ExternalProject_Add(nlopt_src
#   DOWNLOAD_COMMAND wget http://ab-initio.mit.edu/nlopt/nlopt-${VERSION}.tar.gz
#   PATCH_COMMAND tar -xzf ../nlopt-${VERSION}.tar.gz && mv nlopt-${VERSION} ../nlopt_src-build/
#   CONFIGURE_COMMAND nlopt-${VERSION}/configure --with-cxx --without-matlab --prefix=${CATKIN_DEVEL_PREFIX}
#   BUILD_COMMAND make
#   INSTALL_COMMAND make install
# )

set(VERSION v2.7.0)
ExternalProject_Add(nlopt_src    # Name for custom target
# #--Download step--------------
  GIT_REPOSITORY https://github.com/stevengj/nlopt 
  GIT_TAG ${VERSION}
  TIMEOUT 30           # Time allowed for file download operations
# #--Configure step-------------
  BUILD_IN_SOURCE 1
  # CONFIGURE_COMMAND <SOURCE_DIR>/configure --without-matlab --prefix=<INSTALL_DIR>
  CMAKE_ARGS  -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX}
# #--Build step-----------------
  # BUILD_IN_SOURCE 1         # Use source dir for build dir
  # BUILD_COMMAND cmake -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR> && make
# # #--Install step---------------
  INSTALL_DIR ${CATKIN_DEVEL_PREFIX}           # Installation prefix
  # INSTALL_COMMAND ""
  )

  include_directories(
    ${catkin_INCLUDE_DIRS}
    # ${DIRECTORY_TO_FOREIGN_PROJECT_HEADERS}
)

catkin_package(
    # DEPENDS
    # CATKIN_DEPENDS nlopt_src
    INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
    LIBRARIES nlopt
)
