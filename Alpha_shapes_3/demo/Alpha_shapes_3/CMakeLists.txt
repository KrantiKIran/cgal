# Created by the script cgal_create_cmake_script
# This is the CMake script for compiling a CGAL application.

project (Alpha_shapes_3_Demo)

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)
# Instruct CMake to run moc automatically when needed.
set(CMAKE_AUTOMOC ON)
cmake_minimum_required(VERSION 3.1)
if(NOT POLICY CMP0070 AND POLICY CMP0053)
  # Only set CMP0053 to OLD with CMake<3.10, otherwise there is a warning.
  cmake_policy(SET CMP0053 OLD)
endif()
if(POLICY CMP0071)
  cmake_policy(SET CMP0071 NEW)
endif()

find_package(CGAL COMPONENTS Qt5)
include(${CGAL_USE_FILE})

find_package(Qt5 QUIET COMPONENTS Xml Script OpenGL Svg)

find_package(QGLViewer)

if ( CGAL_FOUND AND CGAL_Qt5_FOUND AND Qt5_FOUND AND QGLVIEWER_FOUND )

  add_definitions(-DQT_NO_KEYWORDS)

 # include(${QT_USE_FILE})

  include_directories (${QGLVIEWER_INCLUDE_DIR})
  include_directories (BEFORE ../../include ./ )

  # ui file, created wih Qt Designer
  qt5_wrap_ui( uis MainWindow.ui )

  # qrc files (resources files, that contain icons, at least)
  qt5_add_resources ( CGAL_Qt5_RESOURCE_FILES ./Alpha_shape_3.qrc )

  add_executable  ( Alpha_shape_3 Alpha_shape_3.cpp MainWindow.cpp Viewer.cpp ${uis}  ${CGAL_Qt5_RESOURCE_FILES} ${CGAL_Qt5_MOC_FILES})
  add_to_cached_list( CGAL_EXECUTABLE_TARGETS Alpha_shape_3 )

  target_link_libraries( Alpha_shape_3 PRIVATE
    CGAL::CGAL CGAL::CGAL_Qt5
    Qt5::OpenGL Qt5::Gui
    ${QGLVIEWER_LIBRARIES} )

  include(${CGAL_MODULES_DIR}/CGAL_add_test.cmake)
  cgal_add_compilation_test(Alpha_shape_3)

else()

  message(STATUS "NOTICE: This demo requires CGAL, the QGLViewer, and Qt5, and will not be compiled.")

endif()
