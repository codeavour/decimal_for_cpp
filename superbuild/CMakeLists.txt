cmake_minimum_required(VERSION 3.30.5)
project(decimal_for_cpp)

include(ExternalProject)

ExternalProject_Add(boost
        GIT_REPOSITORY https://github.com/boostorg/boost.git
        GIT_TAG boost-1.87.0
        CMAKE_ARGS
            -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
            -DCMAKE_INSTALL_PREFIX=${CMAKE_BINARY_DIR}/install
        LOG_DOWNLOAD ON
        LOG_CONFIGURE ON
        LOG_BUILD ON
        LOG_INSTALL ON
)

ExternalProject_Add(decimal_for_cpp
        SOURCE_DIR ${CMAKE_SOURCE_DIR}/..
        DEPENDS boost
        CMAKE_ARGS
            -DCMAKE_INSTALL_PREFIX=${CMAKE_BINARY_DIR}/install
            -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
        LOG_DOWNLOAD ON
        LOG_CONFIGURE ON
        LOG_BUILD ON
        LOG_INSTALL ON
)