cmake_minimum_required(VERSION 3.20)
project(llvm_kaleidoscope)

set(CMAKE_CXX_STANDARD 17)

find_package(LLVM REQUIRED CONFIG)
message(STATUS "LLVM Version: ${LLVM_VERSION}")

#################################################################
# Set gdb
#################################################################
SET(CMAKE_BUILD_TYPE "Debug")
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g2 -ggdb")
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")

message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "Using LLVMConfig.cmake in: ${LLVM_DIR}")
# Set your project compile flags.
# E.g. if using the C++ header files
# you will need to enable C++11 support
# for your compiler.

# include_directories("libs")
# add_definitions(${LLVM_DEFINITIONS})

include_directories(${LLVM_INCLUDE_DIRS})

#################################################################
# Now build our tools
#################################################################
add_executable(llvm_kaleidoscope main.cpp)

#################################################################
# Set Flags
#################################################################
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -fno-rtti")

#################################################################
# Find the libraries that correspond to the LLVM components
# that we wish to use
#################################################################
# llvm_map_components_to_libnames(llvm_libs support core irreader ScalarOpts
#         ExecutionEngine OrcJIT native Analysis RuntimeDyld Object InstCombine mcjit)
llvm_map_components_to_libnames(llvm_libs OrcJIT native)

#################################################################
# Add Dynamic .so library
#################################################################
#link_directories(./bin) #添加动态连接库的路径
#add_library( libhello SHARED IMPORTED )
target_link_libraries(llvm_kaleidoscope ${llvm_libs})

#################################################################
# Add Static .a library
#################################################################
#add_library(mxnet STATIC IMPORTED)
#set_property(TARGET mxnet PROPERTY IMPORTED_LOCATION /path/to/libmxnet.a)
#target_link_libraries(project_name mxnet ) #添加libmxnet.a