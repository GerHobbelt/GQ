# Build script for GQ written by Henri Hyyryläinen
# This is meant to be used within a cmake project
cmake_minimum_required(VERSION 3.0)
project("GQ")

set(PROJECT_VERSION 0.0.1-dev)

if(UNIX)

  # C++17
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -Wall")

  configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/pkg-config/GQ.pc.in"
    "${CMAKE_CURRENT_BINARY_DIR}/pkg-config/GQ.pc" @ONLY
  )

  install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/pkg-config/GQ.pc"
    DESTINATION "${CMAKE_INSTALL_PREFIX}/lib/pkgconfig"
  )

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

set_target_properties(GQ PROPERTIES PUBLIC_HEADER "${HEADERS}")

install(
  TARGETS GQ
  LIBRARY DESTINATION lib
  PUBLIC_HEADER DESTINATION include/GQ
)
