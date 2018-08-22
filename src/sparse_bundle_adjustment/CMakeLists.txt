cmake_minimum_required(VERSION 2.8.3)
project(sparse_bundle_adjustment)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${PROJECT_SOURCE_DIR}/CMakeModules/")

find_package(catkin REQUIRED)
find_package(Eigen3 REQUIRED)

include_directories(
	include/${PROJECT_NAME}
	${catkin_INCLUDE_DIRS} 
	${EIGEN3_INCLUDE_DIRS}
	${EIGEN_INCLUDE_DIRS}
	/usr/include/eigen3/
	)

add_definitions(${EIGEN3_DEFINITIONS})

INCLUDE_DIRECTORIES(include)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES sba
)

add_definitions(-DSBA_CHOLMOD)

# SBA library
add_library(sba src/sba.cpp src/spa.cpp src/spa2d.cpp src/csparse.cpp src/proj.cpp src/node.cpp src/sba_file_io.cpp)
#rosbuild_add_compile_flags(sba ${SSE_FLAGS})
target_link_libraries(sba blas lapack cholmod cxsparse)

install(DIRECTORY include/sparse_bundle_adjustment/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})

install(TARGETS sba
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})
