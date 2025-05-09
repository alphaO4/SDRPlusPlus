cmake_minimum_required(VERSION 3.13)
project(tetra_demodulator)

set(CMAKE_INSTALL_PREFIX "/usr/")
include(GNUInstallDirs)

file(GLOB SRC LIST_DIRECTORIES false 
    "src/*.cpp"
    "src/*.c"
    "src/dsp/*.cpp"
    "src/decoder/src/*.cpp"
    "src/decoder/src/*.c"
    "src/decoder/src/crypto/*.cpp"
    "src/decoder/src/crypto/*.c"
    "src/decoder/src/lower_mac/*.cpp"
    "src/decoder/src/lower_mac/*.c"
    "src/decoder/src/phy/*.cpp"
    "src/decoder/src/phy/*.c"
    "src/decoder/codec/c-code/*.c"
    "src/decoder/codec/c-code/*.h"
    )

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -march=native")

if (NOT SDRPP_MODULE_CMAKE)
    set(SDRPP_MODULE_CMAKE "/Users/runner/work/SDRPlusPlus/sdrpp_module.cmake")
endif ()

# Remove the -std=c++17 flag
string(REPLACE "-std=c++17" "" SDRPP_COMPILER_FLAGS "${SDRPP_COMPILER_FLAGS}")
string(REGEX REPLACE "-std=c\\+\\+[0-9]+" "" SDRPP_COMPILER_FLAGS "${SDRPP_COMPILER_FLAGS}")
set(SDRPP_MODULE_COMPILER_FLAGS "${SDRPP_COMPILER_FLAGS}")

# Include the SDR++ module file
include(${SDRPP_MODULE_CMAKE})


if(MSVC)
  # target_compile_options(tetra_demodulator PRIVATE /W4 /WX)
else()
  # target_compile_options(tetra_demodulator PRIVATE -Wall -Wextra)
endif()


target_include_directories(tetra_demodulator PRIVATE 
    "src/" 
    "src/decoder/src" 
    "src/decoder/codec" 
    "src/decoder/codec/c-code"
)
