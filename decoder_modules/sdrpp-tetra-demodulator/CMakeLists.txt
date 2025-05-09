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


if (NOT SDRPP_MODULE_CMAKE)
    set(SDRPP_MODULE_CMAKE "/Users/runner/work/SDRPlusPlus/sdrpp_module.cmake")
endif ()


# Run patching:

# Define the relative path to your script
set(SCRIPT_PATH "${CMAKE_SOURCE_DIR}/src/decoder/etsi_codec-patches/download_and_patch.sh")

add_custom_target(run_script
    COMMAND ${CMAKE_COMMAND} -E echo "Running script from: ${SCRIPT_PATH}"
    COMMAND ${CMAKE_COMMAND} -E chmod +x ${SCRIPT_PATH}
    COMMAND /bin/sh ${SCRIPT_PATH}
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/src/decoder/etsi_codec-patches
    COMMENT "Executing shell script from the correct path"
)



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

target_include_directories(tetra_demodulator PRIVATE BEFORE "src/" "src/decoder/src" "src/decoder/codec" )
