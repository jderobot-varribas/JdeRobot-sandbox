# Copyright (C) 2015 JDE Developers Team
# Authors:
#   2015 Dec, Victor Arribas <v.arribas.urjc@gmail.com>

project (jderobot-sandbox)
cmake_minimum_required(VERSION 2.8)


## Tools
include(cmake/Tools/check_compiler.cmake)
include(cmake/Tools/find_packages.cmake)


## Dependencies
# alphabetical sorted to handle dependency order
# this means that you must to prepend letter based sort at directory level
file(GLOB_RECURSE CMakeLists_files "cmake/Deps/**/CMakeLists.txt")
list(SORT CMakeLists_files)
foreach(CMakeLists_file ${CMakeLists_files})
    message(STATUS "${CMakeLists_file}")
    include(${CMakeLists_file})
endforeach()


## Code (user code)
# alphabetical sorted to handle dependency order
file(GLOB_RECURSE CMakeLists_files RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "src/**/CMakeLists.txt")
list(SORT CMakeLists_files)
if (CMakeLists_files)
    message(STATUS "Found following components to build:")
    foreach(CMakeLists_file ${CMakeLists_files})
        get_filename_component(CMakeLists_dir ${CMakeLists_file} PATH)
        get_filename_component(name ${CMakeLists_dir} NAME)
        message(STATUS "\t - ${name}")
        add_subdirectory(${CMakeLists_dir} EXCLUDE_FROM_ALL)
    endforeach()
endif()

