cmake_minimum_required(VERSION 3.7)
project(ctest-tests)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_CXX_FLAGS "-Wall -Wextra -Wpedantic -Wconversion -Wunreachable-code -Wold-style-cast -fprofile-arcs -ftest-coverage")
set(CMAKE_C_FLAGS "-Wall -Wextra -Wpedantic -Wconversion -Wunreachable-code -Wold-style-cast -fprofile-arcs -ftest-coverage")

include(GoogleTest)

set (TESTS
        fibonachi_test.h
        tst_stdout.h
        tst_test1.h
        )

include_directories(${PROJECT_NAME} PUBLIC ../app)

set (EXT_HEADERS

        ../app/myfunc.h
        )

set (EXT_SOURCES
        ../app/myfunc.c
        )

add_definitions(-DINPUTDIR="${PROJECT_SOURCE_DIR}/input")

add_executable(${PROJECT_NAME} main.cpp ${TESTS} ${EXT_HEADERS} ${EXT_SOURCES})

target_link_libraries(${PROJECT_NAME} gtest gtest_main gmock pthread)

# оценка покрытия кода тестами
target_link_libraries(${PROJECT_NAME} gcov)

gtest_add_tests(TARGET      ${PROJECT_NAME}
        TEST_SUFFIX .noArgs
        TEST_LIST   noArgsTests
        )
gtest_add_tests(TARGET      ${PROJECT_NAME}
        EXTRA_ARGS  --someArg someValue
        TEST_SUFFIX .withArgs
        TEST_LIST   withArgsTests
        )
