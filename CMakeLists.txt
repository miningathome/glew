project (glew C)

cmake_minimum_required(VERSION 2.8)

option(BUILD_SHARED_LIBS "Build shared libraries" OFF)
option(BUILD_STATIC_LIBS "Build static libraries" ON)

# Unify make and MSVC builds (single output directory; get rid of Debug/ Release/)
include (${CMAKE_CURRENT_SOURCE_DIR}/cmake/unify.cmake)

include_directories(include)

# This cmake file builds only 
set (SRC src/glew.c)
set (HDR include/GL/glew.h include/GL/wglew.h)
list (APPEND ADDITIONAL "")

if (WIN32)
	list (APPEND ADDITIONAL old_build/glew.rc)
	list(APPEND platformlibs opengl32)
	add_definitions(-DWIN32_LEAN_AND_MEAN)
endif()

if (BUILD_SHARED_LIBS)
	add_definitions(-DGLEW_BUILD)
	set(libtype SHARED)
elseif (BUILD_STATIC_LIBS)
	add_definitions(-DGLEW_STATIC)
	set(libtype STATIC)
endif()

add_library(glew ${libtype} ${ADDITIONAL} ${SRC} ${HDR})

if (BUILD_SHARED_LIBS)
	target_link_libraries(glew ${platformlibs})
endif()

if (MSVC)
	add_definitions(-DVC_EXTRALEAN)
endif()

# -- INSTALL

install(DIRECTORY include/GL
	DESTINATION include)

install(TARGETS glew EXPORT glewExport
	RUNTIME DESTINATION bin COMPONENT main
	LIBRARY DESTINATION lib COMPONENT main
	ARCHIVE DESTINATION lib COMPONENT main)