
add_executable(gem
  gem_com.f90
  gem_equil.f90
  gem_main.f90
  gem_outd.f90
  gem_fcnt.f90
  gem_fft_wrapper.f90
  gem_gkps_adi.f90
  ppush.f90
  cpush.f90
  grid1.f90
  gem_pputil.f90
  )

target_link_libraries(gem PRIVATE dfftpack LAPACK::LAPACK MPI::MPI_Fortran)

if(CMAKE_Fortran_COMPILER_ID STREQUAL "GNU")
  target_compile_options(gem PRIVATE "$<$<COMPILE_LANGUAGE:Fortran>:-cpp>")
elseif(CMAKE_Fortran_COMPILER_ID STREQUAL "Intel")
  target_compile_options(gem PRIVATE "$<$<COMPILE_LANGUAGE:Fortran>:-fpp>")
else()
  message(FATAL_ERROR "don't know compiler flags to turn on preprocessing of .f90 files")
endif()

add_subdirectory(dfftpack)
