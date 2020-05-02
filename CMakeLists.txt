
cmake_minimum_required(VERSION 3.13 FATAL_ERROR)

# This project isn't really using C, but we list it as a workaround
# for FindBLAS.
project(GEM Fortran C)

if (NOT CMAKE_BUILD_TYPE)
  message(STATUS "CMAKE_BUILD_TYPE was not set. Setting to RelWithDebInfo.")
  set(CMAKE_BUILD_TYPE "RelWithDebInfo" CACHE STRING "Choose the type of build." FORCE)
endif()

find_package(MPI REQUIRED)
find_package(LAPACK REQUIRED)

if (NOT OpenACC_ACCEL_TARGET)
  set(OpenACC_ACCEL_TARGET nvidia::cc70) # Volta on Summit
endif()
find_package(OpenACC REQUIRED)

if (NOT TARGET LAPACK::LAPACK)
  add_library(LAPACK::LAPACK INTERFACE IMPORTED)
  set_property(TARGET LAPACK::LAPACK PROPERTY
    INTERFACE_LINK_LIBRARIES ${LAPACK_LINKER_FLAGS} ${LAPACK_LIBRARIES})
endif()

if (CMAKE_Fortran_COMPILER_ID STREQUAL "GNU")
  set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -ffree-line-length-none")
elseif (CMAKE_Fortran_COMPILER_ID STREQUAL "PGI")
  set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -Minfo=accel")
endif()

add_subdirectory(code)


