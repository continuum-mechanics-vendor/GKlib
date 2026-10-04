cmake_minimum_required(VERSION 3.26)

if(NOT CMAKE_BUILD_TYPE AND NOT DEFINED ENV{CMAKE_BUILD_TYPE})
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type")
endif()

project(GKlib LANGUAGES C VERSION 1.0.0)

enable_testing()

include(GNUInstallDirs)

option(BUILD_TESTING "Build tests")

option(BUILD_SHARED_LIBS "Build shared libraries (.dll/.so) instead of static ones (.lib/.a)")

if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT AND GKlib_IS_TOP_LEVEL)
  set(CMAKE_INSTALL_PREFIX "${PROJECT_BINARY_DIR}/local" CACHE PATH "install prefix" FORCE)
endif()

set(CMAKE_C_STANDARD 99)

message(STATUS "${PROJECT_NAME} ${PROJECT_VERSION} CMake ${CMAKE_VERSION} Arch: ${CMAKE_SYSTEM_PROCESSOR} install prefix: ${CMAKE_INSTALL_PREFIX}")

set(GKLIB_PATH ${PROJECT_SOURCE_DIR})

include(GKlibSystem.cmake)

add_subdirectory(src)

if(BUILD_TESTING)
  add_subdirectory(test)
endif()

install(TARGETS GKlib EXPORT ${PROJECT_NAME}-targets)

# must have trailing slash or it makes a vestigial subdir
install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ TYPE INCLUDE
  FILES_MATCHING PATTERN "gk_*.h")
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/include/GKlib.h TYPE INCLUDE)

include(cmake/install.cmake)

