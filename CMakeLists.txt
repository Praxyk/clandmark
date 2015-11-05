########################################################################
# CMake configuration
########################################################################
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
PROJECT(clandmark CXX)

SET(clandmark_VERSION_MAJOR 1)
SET(clandmark_VERSION_MINOR 5)
SET(clandmark_VERSION ${clandmark_VERSION_MAJOR}.${clandmark_VERSION_MINOR})

SET(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/Modules;${CMAKE_MODULE_PATH}")

########################################################################
# Global flags
########################################################################
IF(CMAKE_COMPILER_IS_GNUCXX OR "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wpedantic")
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wpedantic")
ENDIF(CMAKE_COMPILER_IS_GNUCXX OR "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")

########################################################################
# Create uninstall target
########################################################################
CONFIGURE_FILE(
    ${CMAKE_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake
@ONLY)

ADD_CUSTOM_TARGET(uninstall
    ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake
)

####################################################################
# Set install subdirectories
####################################################################
IF(NOT DEFINED LIB_SUFFIX AND REDHAT AND CMAKE_SYSTEM_PROCESSOR MATCHES "64$")
    SET(LIB_SUFFIX 64)
ENDIF()
SET(LIB_SUFFIX ${LIB_SUFFIX} CACHE STRING "lib directory suffix")
SET(RUNTIME_DIR bin)
SET(LIBRARY_DIR lib${LIB_SUFFIX})
SET(INCLUDE_DIR include)

ADD_SUBDIRECTORY(libclandmark)

####################################################################
# Final Display
####################################################################
MESSAGE(STATUS "")
MESSAGE(STATUS "Building CLandmark.")
MESSAGE(STATUS " - Build type: ${CMAKE_BUILD_TYPE}")
MESSAGE(STATUS " - Compiler: ${CMAKE_CXX_COMPILER_ID} ${CMAKE_CXX_COMPILER_VERSION}")
MESSAGE(STATUS "")
