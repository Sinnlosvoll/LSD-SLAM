cmake_minimum_required(VERSION 2.4.6)
project(lsd_slam_viewer)

# Set the build type. Options are:
#  Coverage : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug : w/ debug symbols, w/o optimization
#  Release : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel : w/o debug symbols, w/ optimization, stripped binaries
set(CMAKE_BUILD_TYPE Release)

ADD_SUBDIRECTORY(${PROJECT_SOURCE_DIR}/thirdparty/Sophus)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/cmake)

find_package(cmake_modules REQUIRED)

set(QGLVIEWER QGLViewer-qt4)


find_library(QGLVIEWER_LIBRARY_RELEASE
    NAMES QGLViewer-qt4 QGLViewer2 QGLViewer2
    HINTS
        #--- Windows
        C:/Developer/lib
        #--- Linux
        /usr/lib
        /usr/local/lib
        /usr/lib/x86_64-linux-gnu
        #--- Mac
        /Library/Frameworks
        #--- Environment
        ENV QGLVIEWERROOT
        ENV LD_LIBRARY_PATH
        ENV LIBRARY_PATH
    PATH_SUFFIXES
        QGLViewer
        QGLViewer/release)

if(QGLVIEWER_LIBRARY_RELEASE)
    if(QGLVIEWER_LIBRARY_DEBUG)
        set(QGLVIEWER_LIBRARIES_ optimized ${QGLVIEWER_LIBRARY_RELEASE} debug ${QGLVIEWER_LIBRARY_DEBUG})
    else()
        set(QGLVIEWER_LIBRARIES_ ${QGLVIEWER_LIBRARY_RELEASE})
    endif()
    set(QGLVIEWER_LIBRARIES ${QGLVIEWER_LIBRARIES_} CACHE FILEPATH "The QGLViewer library")
endif()

#message(STATUS "QGLVIEWER_INCLUDE_DIR: ${QGLVIEWER_INCLUDE_DIR}")
#message(STATUS "QGLVIEWER_LIBRARIES: ${QGLVIEWER_LIBRARIES}")

IF(QGLVIEWER_INCLUDE_DIR AND QGLVIEWER_LIBRARIES)
   SET(QGLVIEWER_FOUND TRUE)
ENDIF()
set(QGLViewer_FIND_REQUIRED true)

IF(QGLVIEWER_FOUND)
    IF(NOT CMAKE_FIND_QUIETLY)
        MESSAGE(STATUS "#-#-#-# Found QGLViewer: ${QGLVIEWER_LIBRARIES}")
    ENDIF()
ELSE()
    IF(QGLViewer_FIND_REQUIRED)
        # MESSAGE(FATAL_ERROR "#-#-#-# Could not find QGLViewer")
    ENDIF()
ENDIF()








find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  dynamic_reconfigure
  sensor_msgs
  roscpp
  rosbag
  message_generation
  roslib
)

find_package(OpenGL REQUIRED)
set(QT_USE_QTOPENGL TRUE)
set(QT_USE_QTXML TRUE)
find_package(QGLViewer REQUIRED)
find_package(Eigen REQUIRED)
find_package(OpenCV REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)

include_directories(${QGLVIEWER_INCLUDE_DIR}
		    ${catkin_INCLUDE_DIRS} 
		    ${EIGEN_INCLUDE_DIR}
		    ${QT_INCLUDES} )

# SSE flags
set(CMAKE_CXX_FLAGS
   "${CMAKE_CXX_FLAGS} -march=native -Wall -std=c++0x"
)

add_message_files(DIRECTORY msg FILES keyframeMsg.msg keyframeGraphMsg.msg)
generate_messages(DEPENDENCIES)

generate_dynamic_reconfigure_options(
  cfg/LSDSLAMViewerParams.cfg
)


# Sources files
set(SOURCE_FILES         
  src/PointCloudViewer.cpp
  src/KeyFrameDisplay.cpp
  src/KeyFrameGraphDisplay.cpp
  src/settings.cpp
)

set(HEADER_FILES     
  src/PointCloudViewer.h
  src/KeyFrameDisplay.h
  src/KeyFrameGraphDisplay.h
  src/settings.h
)

include_directories(
  ${PROJECT_SOURCE_DIR}/thirdparty/Sophus
)  

find_package(OpenCV 3 REQUIRED)
include_directories(
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
)

FIND_PACKAGE(QGLViewer REQUIRED)
INCLUDE_DIRECTORIES(${QGLVIEWER_INCLUDE_DIR})


add_executable(viewer src/main_viewer.cpp ${SOURCE_FILES} ${HEADER_FILES})
target_link_libraries(viewer ${QGLViewer_LIBRARIES}
           ${QGLVIEWER_LIBRARY} 
           ${catkin_LIBRARIES}
           ${OpenCV_LIBRARIES}
           ${Boost_LIBRARIES}
           ${QT_LIBRARIES}
           GL glut GLU
		     )

#add_executable(videoStitch src/main_stitchVideos.cpp)
#target_link_libraries(viewer ${QGLViewer_LIBRARIES}
#			     ${QGLVIEWER_LIBRARY}
#			     ${catkin_LIBRARIES}
#			     ${QT_LIBRARIES}
#			     GL glut GLU
#		      )

add_dependencies(viewer lsd_slam_viewer_generate_messages_cpp)
