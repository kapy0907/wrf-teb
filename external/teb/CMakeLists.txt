# WRF-TEB (https://github.com/teb-model/wrf-teb).
# Copyright 2018-2020 D. Meyer. Licensed under the MIT License.

include(ExternalProject)

if (DEFINED TEB_DIR)
    set(dir_options
        SOURCE_DIR ${TEB_DIR}
        BUILD_ALWAYS 1
        )
else()
    set(dir_options
        GIT_REPOSITORY https://github.com/teb-model/teb
        GIT_TAG origin/master
        )
endif()

ExternalProject_Add(teb-project
    ${dir_options}
    INSTALL_COMMAND ""
    CMAKE_ARGS
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
        -DCMAKE_Fortran_COMPILER=${CMAKE_Fortran_COMPILER}
        -DCMAKE_Fortran_FLAGS_DEBUG=${CMAKE_Fortran_FLAGS_DEBUG}
        -DCMAKE_Fortran_FLAGS_RELEASE=${CMAKE_Fortran_FLAGS_RELEASE}
        -DCMAKE_Fortran_FLAGS=${CMAKE_Fortran_FLAGS}
        -DUSE_REAL8=OFF
)

ExternalProject_Get_Property(teb-project binary_dir)

add_library(teb INTERFACE)
add_dependencies(teb teb-project)
target_include_directories(teb INTERFACE ${binary_dir}/mod)
target_link_libraries(teb INTERFACE ${binary_dir}/${CMAKE_STATIC_LIBRARY_PREFIX}teb${CMAKE_STATIC_LIBRARY_SUFFIX})
