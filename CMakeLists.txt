cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# OpenCV
FIND_PACKAGE( OpenCV REQUIRED )
include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIBRARY_DIRS})
link_libraries(${OpenCV_LIBRARIES})

# PCL
find_package(PCL REQUIRED)
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
link_libraries(${PCL_LIBRARIES})

# Octomap
find_package(octomap REQUIRED)
include_directories(${OCTOMAP_INCLUDE_DIRS})
link_directories(${OCTOMAP_LIBRARY_DIRS})
link_libraries(${OCTOMAP_LIBRARIES})

#g2o
#find_package(libg2o REQUIRED)
#include_directories(${libg2o_INCLUDE_DIRS})
#link_directories(${libg2o_LIBRARY_DIRS})
#link_libraries(${libg2o_LIBRARIES})


# TBB
# TBB will be removed later
# find_package(tbb REQUIRED)
# include_directories(${TBB_INCLUDE_DIRS})
# link_directories(${TBB_LIBRARY_DIRS})
# link_libraries(${TBB_LIBRARES})

                                             
                                             
#g2o
#set(G2O_LIBRARIES 
#    g2o_core 
#    g2o_stuff
#    g2o_solver_cholmod 
#    g2o_solver_csparse 
#    g2o_types_slam3d
#    cholmod
#)

# boost
rosbuild_add_boost_directories()

rosbuild_add_library(keyframe_livemapper
    src/apps/keyframe_livemapper.cpp
    src/apps/rgbdtools_graph_detector.cpp
    src/apps/rgbdtools_graph_solver.cpp
    src/apps/octomap_handler.cpp
)

target_link_libraries(keyframe_livemapper
    rgbdtools
    ${OpenCV_LIBRARIES}
    tbb
)

rosbuild_add_executable(keyframe_livemapper_node
    src/node/keyframe_livemapper_node.cpp
)

target_link_libraries(keyframe_livemapper_node
    keyframe_livemapper
)
