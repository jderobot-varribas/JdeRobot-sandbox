## The hard way (no help)

set(libname JderobotInterfaces)

find_path(main_include NAMES common.h PATHS ENV C++LIB ENV PATH PATH_SUFFIXES jderobot)
find_library(main_lib NAMES ${libname} PATHS ENV C++LIB PATH PATH_SUFFIXES jderobot)

if (main_include AND main_lib)
    get_filename_component(main_lib_dir ${main_lib} PATH)
    message(STATUS "${libname} FOUND")
    set(${libname}_FOUND 1)
    set(${libname}_INCLUDE_DIRS ${main_include})
    set(${libname}_LIBRARY_DIRS ${main_lib_dir})
    set(${libname}_LIBRARIES ${main_lib})
else()
    message(WARNING "${libname} NOT FOUND")
endif()


if(${libname}_FOUND AND INJECT_JDE_LIBS)
    include_directories(${${libname}_INCLUDE_DIRS})
    link_directories(${${libname}_LIBRARY_DIRS})
endif()
