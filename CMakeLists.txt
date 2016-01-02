# Copyright (C) 2015 JDE Developers Team
# Authors:
#   2015 Dec, Victor Arribas <v.arribas.urjc@gmail.com>

project (jderobot-sandbox)
cmake_minimum_required(VERSION 2.8)


## Tools
include(cmake/Tools/check_compiler.cmake)
include(cmake/Tools/find_packages.cmake)


## Dependencies
# allow Deps to populate
#  * include_directories
#  * link_directories
# for JdeRobot libraries
set(INJECT_JDE_LIBS ON)
# for 3rd party libraries
set(INJECT_DEPS_LIBS OFF)

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
    string(REGEX REPLACE "//" "/" list ${CMakeLists_files})
    string(REGEX REPLACE "/CMakeLists.txt" "" list ${list})
    string(REGEX REPLACE "src/" "\n\t- " list ${list})
    message(STATUS "Found following components to build: ${list}\n")

    foreach(CMakeLists_file ${CMakeLists_files})
        get_filename_component(CMakeLists_dir ${CMakeLists_file} PATH)
        string(REGEX REPLACE "src/" "" name ${CMakeLists_dir})
        message(STATUS "Configuring '${name}'")
        add_subdirectory(${CMakeLists_dir})
    endforeach()
endif()

