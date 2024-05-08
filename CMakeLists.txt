# SPDX-FileCopyrightText: 2024 Marian Sauer
#
# SPDX-License-Identifier: BSD-3-Clause

cmake_minimum_required(VERSION 3.16)

#
# This file is generated only once,
# and is not re-generated if converter is called multiple times.
#
# User is free to modify the file as much as necessary
#

# Setup compiler settings
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)


# Define the build type
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

# Set the project name
set(CMAKE_PROJECT_NAME CanShield)

# Include toolchain file
include("cmake/gcc-arm-none-eabi.cmake")

# Enable compile command to ease indexing with e.g. clangd
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

# Enable CMake support for ASM and C languages
enable_language(C ASM)

# Core project settings
project(${CMAKE_PROJECT_NAME})
message("Build type: " ${CMAKE_BUILD_TYPE})

# Create an executable object type
add_executable(main_tx)
target_sources(main_tx INTERFACE
    Core/Src/main_tx.c
    startup_stm32f103xb.s
)

add_executable(main_rx)
target_sources(main_rx INTERFACE
    Core/Src/main_rx.c
    startup_stm32f103xb.s
)

# Add STM32CubeMX generated sources
add_subdirectory(cmake/stm32cubemx)

# Add linked libraries
target_link_libraries(main_tx
    stm32cubemx
)

target_link_libraries(main_rx
    stm32cubemx
)

# generate binary files for drag and drop programming
add_custom_target(bin_file_tx ALL COMMAND arm-none-eabi-objcopy $<TARGET_FILE:main_tx> -O binary -S main_tx.bin)
add_custom_target(bin_file_rx ALL COMMAND arm-none-eabi-objcopy $<TARGET_FILE:main_rx> -O binary -S main_rx.bin)