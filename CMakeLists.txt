cmake_minimum_required(VERSION 3.12 FATAL_ERROR)

set(WAVELIB_VERSION "1.0.0" CACHE STRING "Wavelib version" FORCE)

set(PROJECT_NAME wavelib)
project(${PROJECT_NAME} VERSION ${WAVELIB_VERSION} LANGUAGES CXX C)

# src root path
set(WAVELIB_SRC_ROOT ${PROJECT_SOURCE_DIR} CACHE PATH "Wavelib source root")
# binary output by default
set(COMMON_BIN_PATH ${CMAKE_BINARY_DIR}/Bin)
set(LIBRARY_OUTPUT_PATH ${COMMON_BIN_PATH}/${CMAKE_BUILD_TYPE})
set(EXECUTABLE_OUTPUT_PATH ${COMMON_BIN_PATH}/${CMAKE_BUILD_TYPE})
# set where to find additional cmake modules if any
set(CMAKE_MODULE_PATH ${WAVELIB_SRC_ROOT}/cmake ${CMAKE_MODULE_PATH})

message(">>> Building Wavelib version: ${WAVELIB_VERSION}")
message(">>> EXECUTABLE_OUTPUT_PATH = ${EXECUTABLE_OUTPUT_PATH}")

option(BUILD_UT "Enable Unit test" ON)

# cleanup prefix lib for Unix-like OSes
set(CMAKE_SHARED_MODULE_PREFIX)

# install target to this folder by default
set(WAVELIB_BINARY_DIR ${WAVELIB_SRC_ROOT}/bin)
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(CMAKE_INSTALL_PREFIX "${WAVELIB_BINARY_DIR}" CACHE PATH "default install path" FORCE)
endif()

# make include globaly visible
set(PROJECT_WIDE_INCLUDE ${WAVELIB_SRC_ROOT}/include)
include_directories(${PROJECT_WIDE_INCLUDE})

include_directories(${COMMON_BIN_PATH})

if(BUILD_UT)
    include(CTest)
    enable_testing()
    add_subdirectory(unitTests)
endif()

add_subdirectory(src)
add_subdirectory(auxiliary)
add_subdirectory(test) 

install(EXPORT wavelib
  DESTINATION lib/cmake/wavelib
  NAMESPACE wavelib::
  FILE wavelib.cmake
)

set(INCLUDE_INSTALL_DIR "include")

include(CMakePackageConfigHelpers)
configure_package_config_file(${CMAKE_SOURCE_DIR}/cmake/Config.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/wavelib-config.cmake"
  INSTALL_DESTINATION lib/cmake/wavelib
  PATH_VARS INCLUDE_INSTALL_DIR
)

install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/wavelib-config.cmake
  DESTINATION lib/cmake/wavelib
)
