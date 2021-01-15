# Build script for GQ written by Henri Hyyryläinen
# This is meant to be used within a cmake project
cmake_minimum_required(VERSION 3.0)

if(UNIX)
  
  # C++14
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -Wall -Wl,--no-undefined -Wl,--no-allow-shlib-undefined")
  
endif(UNIX)

find_package(Boost 1.69.0 REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(GUMBO REQUIRED gumbo)

include_directories(${GUMBO_INCLUDE_DIRS})
link_directories(${GUMBO_LIBRARY_DIRS})

file(GLOB_RECURSE HEADERS "src/*.hpp")
file(GLOB_RECURSE SOURCES "src/*.cpp")

add_library(GQ SHARED "${HEADERS}" "${SOURCES}")

target_link_libraries(GQ ${GUMBO_LIBRARIES})


  
