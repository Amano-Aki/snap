cmake_minimum_required(VERSION 3.1)

project(snap)
enable_language(CXX)
set(CMAKE_CXX_STANDARD 98)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    add_compile_options(-Wall -fPIC -O3 -DNDEBUG -fopenmp -fmax-errors=1 -w)
    link_libraries(rt)
elseif(CMAKE_SYSTEM_NAME STREQUAL "CYGWIN")
    add_compile_options(-Wall -O3 -fopenmp -DNDEBUG -D__STDC_LIMIT_MACROS -fmax-errors=1 -w)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    # Some unix functions are assumed to be available in many sources, but not defined on windows.
    # This adds declarations in all sources
    add_definitions(/D_CRT_RAND_S)
    set(COMPAT_HEADER_FLAG "/FI${CMAKE_CURRENT_SOURCE_DIR}/cmake/windows_compat.h")
    add_compile_options("$<$<CONFIG:RELEASE>:/MT>" "$<$<CONFIG:DEBUG>:/MTd>" /w ${COMPAT_HEADER_FLAG})
    # And the definition is added to all binaries.
    add_library(windowsCompat
        ${CMAKE_CURRENT_SOURCE_DIR}/cmake/windows_compat.h
        ${CMAKE_CURRENT_SOURCE_DIR}/cmake/windows_compat.cpp
    )
    link_libraries(windowsCompat)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -O3 -DNDEBUG -Wno-unknown-pragmas -fmax-errors=1 -w")
    if(CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
        add_definitions(-DNOMP)
    else()
        add_compile_options(-fopenmp)
    endif(CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
endif(CMAKE_SYSTEM_NAME STREQUAL "Linux")

set(CMAKE_INCLUDE_CURRENT_DIR ON)

option(BUILD_SNAP_EXECUTABLES "You may want to turn this off if you only use SNAP as a library." ON)

add_compile_definitions(SW_NOABORT)

add_subdirectory(glib-core)
set(SNAPADV_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/snap-adv")
set(SNAPADV_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/snap-adv")
set(SNAPEXP_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/snap-exp")
set(SNAPEXP_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/snap-exp")

add_subdirectory(snap-core)
add_subdirectory(snap-adv)

if (BUILD_SNAP_EXECUTABLES)
    add_subdirectory(examples)
    add_subdirectory(test)
    add_subdirectory(tutorials)
endif (BUILD_SNAP_EXECUTABLES)