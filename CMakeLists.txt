cmake_minimum_required(VERSION 3.10)
project(NooDS)

# Lowering the optimization level on GCC as building with -O3 causes the
# function 'writeFallback' to not get exported (maybe a bug with GCC)
if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O2")
endif()

file(GLOB SOURCES "src/*.cpp")

add_library(${PROJECT_NAME} ${SOURCES})

# Windows doesn't have the header unistd.h
if(WIN32)
    target_compile_definitions(${PROJECT_NAME} PRIVATE WINDOWS NO_FDOPEN)
endif()

target_include_directories(${PROJECT_NAME}
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src/include>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)
