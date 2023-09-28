cmake_minimum_required(VERSION 3.5)
project(checkers)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "-Wall")

if(${CMAKE_HOST_SYSTEM_NAME} STREQUAL  "Linux")
    set(SYSTEM_DIR linux)
else()
    set(SYSTEM_DIR mingw)
endif()

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/build/${SYSTEM_DIR})

include_directories(${CMAKE_CURRENT_LIST_DIR}/checkers/include)
link_directories(${CMAKE_CURRENT_LIST_DIR}/lib/${SYSTEM_DIR})

file(GLOB_RECURSE SOURCES ${CMAKE_CURRENT_LIST_DIR}/checkers/source/*.cpp)
list(REMOVE_ITEM SOURCES ${CMAKE_CURRENT_LIST_DIR}/checkers/source/main.cpp)

file(GLOB_RECURSE HEADERS ${CMAKE_CURRENT_LIST_DIR}/checkers/include/*.h)
file(GLOB_RECURSE HEADERS_HPP ${CMAKE_CURRENT_LIST_DIR}/checksers/include/*.hpp)

file(GLOB_RECURSE TEST_SOURCES ${CMAKE_CURRENT_LIST_DIR}/test/*.cpp)
file(GLOB_RECURSE TEST_HEADERS ${CMAKE_CURRENT_LIST_DIR}/test/*.h)

add_executable(checkers ${HEADERS} ${HEADERS_HPP} ${SOURCES} checkers/source/main.cpp)
add_executable(checkers_ut ${HEADERS} ${HEADERS_HPP} ${SOURCES} ${TEST_SOURCES} ${TEST_HEADERS})

if(${CMAKE_HOST_SYSTEM_NAME} STREQUAL  "Linux")
    target_link_libraries(checkers boost_system pthread)
    target_link_libraries(checkers_ut boost_system gtest gtest_main pthread)
else()
    target_link_libraries(checkers boost_system pthread ws2_32)
    target_link_libraries(checkers_ut boost_system gtestd gtest_maind pthread ws2_32)
endif()

