cmake_minimum_required(VERSION 2.8)
add_definitions (-D_GLIBCXX_USE_CXX11_ABI=0)
add_definitions(-std=c++11)
project(BoostPoco)

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

add_executable(demo demo.cpp)
target_link_libraries(demo ${CONAN_LIBS})
