project (glew C)

cmake_minimum_required(VERSION 2.8)

option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

# Unify make and MSVC builds (single output directory; get rid of Debug/ Release/)
include (${CMAKE_CURRENT_SOURCE_DIR}/cmake/unify.cmake)

include_directories(include)

# This cmake file builds only 
set (SRC src/glew.c)
set (HDR include/GL/glew.h include/GL/wglew.h)
list (APPEND ADDITIONAL "")

if (WIN32)
	list (APPEND ADDITIONAL old_build/glew.rc)
	add_definitions(-DWIN32_LEAN_AND_MEAN)
endif()

add_definitions(-DGLEW_BUILD)

if (MSVC)
	add_definitions(-DVC_EXTRALEAN)
endif()

add_library(glew ${ADDITIONAL} ${SRC} ${HDR})