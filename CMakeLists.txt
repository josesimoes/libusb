cmake_minimum_required(VERSION 3.14.0)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/modules")

set(LIBUSB_MAJOR 1)
set(LIBUSB_MINOR 0)
set(LIBUSB_MICRO 23)

project(libusb VERSION ${LIBUSB_MAJOR}.${LIBUSB_MINOR}.${LIBUSB_MICRO})

option(WITH_SHARED "Build a shared library" ON)
option(WITH_STATIC "Build a static library" OFF)
option(WITH_DOCS "Build the documentation" OFF)

option(WITH_DEBUG_LOG "enable debug logging" OFF)

# if debug logging is enabled, by default enable logging
option(WITH_LOGGING "if false, disable all logging" ON)

# enable examples by default if building with maintainer mode
option(WITH_EXAMPLES "build example applications" ${WITH_MAINTAINER_MODE})

option(WITHOUT_PTHREADS "force pthreads to not be used.  if on, then they are used based on detection logic" OFF)

macro(append_compiler_flags)
	foreach(FLAG IN ITEMS ${ARGN})
		set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${FLAG}")
		set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${FLAG}")
	endforeach()
endmacro()

# need these to enable call fixup by the linker
SET(CMAKE_MODULE_LINKER_FLAGS "-Wl,--enable-stdcall-fixup ${CMAKE_SHARED_LINKER_FLAGS}")
SET(CMAKE_SHARED_LINKER_FLAGS "-Wl,--enable-stdcall-fixup ${CMAKE_SHARED_LINKER_FLAGS}")
SET(CMAKE_EXE_LINKER_FLAGS "-Wl,--enable-stdcall-fixup ${CMAKE_EXE_LINKER_FLAGS}")

if (WITH_DOCS)
	add_subdirectory(docs)
endif ()

if (WITH_EXAMPLES)
	add_subdirectory(examples)
endif()

add_subdirectory(libusb)
