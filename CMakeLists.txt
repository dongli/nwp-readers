cmake_minimum_required(VERSION 3.13)

project(nwp-readers LANGUAGES Fortran)

if (CMAKE_Fortran_COMPILER_ID STREQUAL "GNU")
  set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -ffree-line-length-none")
  if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -g -Wall -Wextra -Warray-temporaries -Wconversion -fimplicit-none -fbacktrace -fcheck=all -ffpe-trap=zero,overflow,underflow -finit-real=nan")
  else ()
    set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -ffpe-summary=none -Ofast")
  endif ()
endif ()

set (CMAKE_Fortran_MODULE_DIRECTORY "${CMAKE_BINARY_DIR}")
include_directories(${CMAKE_BINARY_DIR})

add_subdirectory(lib/container)
add_subdirectory(lib/datetime)
add_subdirectory(lib/kdtree)
add_subdirectory(lib/string)
add_subdirectory(lib/flogger)
add_subdirectory(lib/fiona)
add_subdirectory(lib/proj)
add_subdirectory(lib/sprint)

set(sources
  src/calmet_reader_mod.F90
)

add_library(nwpreaders ${sources})
target_link_libraries(nwpreaders
  fortran_container
  fortran_datetime
  fortran_string
  fortran_proj
  kdtree
  flogger
  fiona
  sprint
)

add_executable(calmet_reader_test.exe src/calmet_reader_test.F90)
target_link_libraries(calmet_reader_test.exe nwpreaders)