# ---------------------------------------------------------------------------
#  Copyright (c) 2020 Armin Sobhani <arminms@gmail.com>
#
#  Distributed under the Boost Software License, Version 1.0
#  See accompanying file LICENSE_1_0.txt or copy at
#  http://www.boost.org/LICENSE_1_0.txt
#
# ---------------------------------------------------------------------------

cmake_minimum_required(VERSION 3.9)

include(cmake/BuildType.cmake)

project(BoostLaal CXX)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH true)

find_package(Boost 1.61 REQUIRED)
include_directories(SYSTEM ${Boost_INCLUDE_DIRS})

option(LAAL_BUILD_TESTS "Build the unit tests ?" OFF)
if(${LAAL_BUILD_TESTS})
  enable_testing()
  add_subdirectory(test)
endif()

option(LAAL_BUILD_BENCHMARKS "Build the benchmarks ?" OFF)
if(${LAAL_BUILD_BENCHMARKS})
  find_package(benchmark
    CONFIG
    HINTS $ENV{HOME} /usr/local /opt /opt/local
  )
  include(external/benchmark.cmake)
  add_subdirectory(perf)
endif()

# configure cmake config file
configure_file(
  cmake/BoostLaalConfig.cmake.in
  ${BoostLaal_BINARY_DIR}/BoostLaalConfig.cmake
  @ONLY
)

# install cmake config file
install(
  FILES ${BoostLAAL_BINARY_DIR}/BoostLaalConfig.cmake
  DESTINATION lib/cmake/BoostLaal
)

# install header files
install(DIRECTORY include/boost DESTINATION include/laal)