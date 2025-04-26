cmake_minimum_required(VERSION 3.12)
project(decimal_for_cpp)
include(CTest)

# Disables relying on CMake providing FindBoost.cmake
cmake_policy(SET CMP0167 NEW)

#set(PROJECT_VERSION 3.4.1)

set(Boost_USE_STATIC_LIBS OFF)
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME OFF)

set(CMAKE_CXX_STANDARD 11 CACHE STRING "C++ standard version to use (default is 11)")
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(BUILD_TESTING)
    find_package(Boost COMPONENTS unit_test_framework REQUIRED)

    if(Boost_FOUND)
        include_directories(${Boost_INCLUDE_DIRS} include)
        add_executable(test_runner include/decimal.h
                tests/runner.cpp
                tests/decimalTest.cpp
                tests/decimalTestAbout.cpp
                tests/decimalTestArithmetic.cpp
                tests/decimalTestDiv.cpp
                tests/decimalTestEdgeCases.cpp
                tests/decimalTestModulo.cpp
                tests/decimalTestMult.cpp
                tests/decimalTestMultDiv.cpp
                tests/decimalTestRounding.cpp
                tests/decimalTestRoundOth.cpp
                tests/decimalTestString.cpp
                tests/decimalTestTypeLevel.cpp
                tests/decimalTestUtils.cpp
                tests/decimalTestWithExponent.cpp
                tests/decimalTestUtils.h)
        target_include_directories(test_runner PRIVATE ${BOOST_INCLUDE_DIRS})
    endif()

    enable_testing()
    add_test(test_runner test_runner)
endif()

add_library(decimal_for_cpp INTERFACE)
target_include_directories(decimal_for_cpp INTERFACE
        $<INSTALL_INTERFACE:include>
)

install(DIRECTORY include/ DESTINATION include/decimal_for_cpp)

install(TARGETS decimal_for_cpp
        EXPORT decimal_for_cppTargets
)

include(CMakePackageConfigHelpers)

# write_basic_package_version_file(
#         "${CMAKE_CURRENT_SOURCE_DIR}/cmake/decimal_for_cppConfigVersion.cmake"
#         VERSION ${PROJECT_VERSION}
#         COMPATIBILITY SameMajorVersion
# )

configure_package_config_file(
        "${CMAKE_CURRENT_SOURCE_DIR}/cmake/decimal_for_cppConfig.cmake.in"
        "${CMAKE_CURRENT_SOURCE_DIR}/cmake/decimal_for_cppConfig.cmake"
        INSTALL_DESTINATION lib/cmake/decimal_for_cpp
)

install(FILES
        "${CMAKE_CURRENT_SOURCE_DIR}/cmake/decimal_for_cppConfig.cmake"
#        "${CMAKE_CURRENT_SOURCE_DIR}/cmake/decimal_for_cppConfigVersion.cmake"
        DESTINATION lib/cmake/decimal_for_cpp
)

install(EXPORT decimal_for_cppTargets
        NAMESPACE decimal_for_cpp::
        DESTINATION lib/cmake/decimal_for_cpp
)