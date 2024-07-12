cmake_minimum_required(VERSION 3.5.0)
project(MobitParser VERSION 0.1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CTEST_OUTPUT_ON_FAILURE 1)

include_directories(${PROJECT_SOURCE_DIR}/include)

file(GLOB SOURCES ${PROJECT_SOURCE_DIR}/src/*.cpp)
file(GLOB TEST_SOURCES ${PROJECT_SOURCE_DIR}/tests/*.cpp)

add_library(MobitParser STATIC ${SOURCES})

add_compile_definitions(DEBUG_LOG)

target_include_directories(MobitParser PUBLIC ${PROJECT_SOURCE_DIR}/include)

include(CTest)
enable_testing()

foreach(test_source ${TEST_SOURCES})
    get_filename_component(test_name ${test_source} NAME_WE)
    add_executable(${test_name} ${test_source})
    target_link_libraries(${test_name} PRIVATE MobitParser)
    add_test(NAME ${test_name} COMMAND ${test_name})
endforeach()

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
