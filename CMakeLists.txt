cmake_minimum_required(VERSION 3.16)

project(Graphs)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

include(FetchContent)

FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG        release-1.11.0
)
FetchContent_MakeAvailable(googletest)
add_library(GTest::GTest INTERFACE IMPORTED)
target_link_libraries(GTest::GTest INTERFACE gtest_main)


add_library(
    ${CMAKE_PROJECT_NAME}
        src/source.cpp
)

target_include_directories( 
    ${CMAKE_PROJECT_NAME}    
    
    PUBLIC 
        src/
)

enable_testing()
set(UT_PROJECT "${CMAKE_PROJECT_NAME}_ut")

add_executable(
    ${UT_PROJECT}
        test/test_source.cpp
)

target_link_libraries(
    ${UT_PROJECT}
    
    PRIVATE
        GTest::GTest
        ${CMAKE_PROJECT_NAME}
)

add_test("${CMAKE_PROJECT_NAME}_gtest" ${UT_PROJECT})