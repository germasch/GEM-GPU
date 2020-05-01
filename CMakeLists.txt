
cmake_minimum_required(VERSION 3.13 FATAL_ERROR)

project(GEM Fortran)

if (NOT CMAKE_BUILD_TYPE)
  message(STATUS "CMAKE_BUILD_TYPE was not set. Setting to RelWithDebInfo.")
  set(CMAKE_BUILD_TYPE "RelWithDebInfo")
endif()

find_package(MPI REQUIRED)
find_package(LAPACK REQUIRED)

if (NOT TARGET LAPACK::LAPACK)
  add_library(LAPACK::LAPACK INTERFACE IMPORTED)
  set_property(TARGET LAPACK::LAPACK PROPERTY
    INTERFACE_LINK_LIBRARIES ${LAPACK_LINKER_FLAGS} ${LAPACK_LIBRARIES})
endif()

if (CMAKE_Fortran_COMPILER_ID STREQUAL "GNU")
  set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -ffree-line-length-none")
  set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -fdefault-real-8 -fdefault-double-8")
elseif (CMAKE_Fortran_COMPILER_ID STREQUAL "Intel")
  set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -r8")
else()
  message(FATAL_ERROR "don't know compiler flags for default double precision.")
endif()

add_subdirectory(code)


