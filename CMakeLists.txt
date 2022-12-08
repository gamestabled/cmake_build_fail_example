cmake_minimum_required(VERSION 3.0)
set (CMAKE_CXX_STANDARD 17)
project(Z3DR C CXX ASM)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/cmake)
include(Tools3DS)

find_package(CTRULIB REQUIRED)

find_library(LIBCITRO2D_LIBRARY NAMES libcitro2d.a
          PATHS ${CTRULIB_PATHS}
          PATH_SUFFIXES lib libctru/lib )

add_library(3ds::citro2d STATIC IMPORTED GLOBAL)
set_target_properties(3ds::citro2d PROPERTIES
    IMPORTED_LOCATION "${LIBCITRO2D_LIBRARY}"
    INTERFACE_INCLUDE_DIRECTORIES "${LIBCTRU_INCLUDE_DIR}"
)

find_library(LIBCITRO3D_LIBRARY NAMES libcitro3d.a
          PATHS ${CTRULIB_PATHS}
          PATH_SUFFIXES lib libctru/lib )

add_library(3ds::citro3d STATIC IMPORTED GLOBAL)
set_target_properties(3ds::citro3d PROPERTIES
    IMPORTED_LOCATION "${LIBCITRO3D_LIBRARY}"
    INTERFACE_INCLUDE_DIRECTORIES "${LIBCTRU_INCLUDE_DIR}"
)

file(GLOB_RECURSE SHADERS_FILES
    data/*.pica
)

file(GLOB_RECURSE SOURCE_FILES
    source/main.cpp
)

add_executable(z3dr ${SOURCE_FILES})
target_link_libraries(z3dr PRIVATE 3ds::ctrulib)

add_3dsx_target(z3dr)
