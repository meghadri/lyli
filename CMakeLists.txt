cmake_minimum_required(VERSION 3.1)
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/CMakeModules)
project(lyli C CXX)

set(CMAKE_CXX_FLAGS_DEBUG "-g -Wall -Wstrict-aliasing -Wextra")

################
# Dependencies #
################

# GUI (optional)
find_package(Qt5Core)
find_package(Qt5Widgets)

# minimal dependencies
find_package(JsonCpp REQUIRED 1.4.0)
find_package(OpenCV REQUIRED)
find_package(TBB REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBUSB REQUIRED libusb-1.0>=1.0.0)
find_package(Threads)

# pass the OpenCV version as a define (the included defines are not reliable for OpenCV 2)
add_definitions(-DOPENCV_VERSION=${OpenCV_VERSION_MAJOR})

# pkg-config to find libusbpp-1.0
find_package(PkgConfig REQUIRED)
pkg_check_modules(USBPP REQUIRED usbpp)

##################
# Compiler flags #
##################

set(CMAKE_CXX_STANDARD 14)
add_definitions(${TBB_DEFINITIONS})
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${JsonCpp_INCLUDE_DIRS})
include_directories(${TBB_INCLUDE_DIRS})
include_directories(${LIBUSB_INCLUDE_DIRS})
include_directories(${OpenCV_INCLUDE_DIRS})
include_directories(lib)

#########
# Build #
#########

# liblyli
add_subdirectory(lib)

# ui part
if (Qt5Core_FOUND AND Qt5Widgets_FOUND)
	add_subdirectory(ui)
else (Qt5Core_FOUND AND Qt5Widgets_FOUND)
	message(ERROR Qt5 not found)
endif()

# aditional tools
add_subdirectory(tools)
