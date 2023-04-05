cmake_minimum_required(VERSION 3.16)

project(ProjectTemplate)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fprofile-arcs -ftest-coverage")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fprofile-arcs -ftest-coverage")

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
        gcov
)

add_test("${CMAKE_PROJECT_NAME}_gtest" ${UT_PROJECT})

set(GCOV_FILES_PATH "CMakeFiles/${CMAKE_PROJECT_NAME}.dir/src")
add_custom_target(coverage
    DEPENDS ${UT_PROJECT}
    COMMAND ./${UT_PROJECT}
    COMMAND mkdir -p coverage
    COMMAND gcov ${GCOV_FILES_PATH}/source.cpp.gcno
    COMMAND lcov --capture --directory ${GCOV_FILES_PATH} --output-file coverage/coverage.info
    COMMAND genhtml coverage/coverage.info --output-directory coverage
)
