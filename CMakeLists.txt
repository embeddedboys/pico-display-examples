cmake_minimum_required(VERSION 3.12)

# Pull in SDK (must be before project)
include(pico_sdk_import.cmake)
include(pico_extras_import_optional.cmake)

project(pico_display_examples C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
SET(CMAKE_EXE_LINKER_FLAGS_DEBUG "${CMAKE_EXE_LINKER_FLAGS_DEBUG} -Wl,--print-memory-usage")
SET(CMAKE_EXE_LINKER_FLAGS_RELEASE "${CMAKE_EXE_LINKER_FLAGS_RELEASE} -Wl,--print-memory-usage")

if (PICO_SDK_VERSION_STRING VERSION_LESS "2.1.0")
    message(FATAL_ERROR "Raspberry Pi Pico SDK version 2.1.0 (or later) required. Your version is ${PICO_SDK_VERSION_STRING}")
endif()

set(PICO_DISPLAY_EXAMPLES_PATH ${PROJECT_SOURCE_DIR})

# If you want debug output from USB (pass -DPICO_STDIO_USB=1) this ensures you don't lose any debug output while USB is set up
if (NOT DEFINED PICO_STDIO_USB_CONNECT_WAIT_TIMEOUT_MS)
    set(PICO_STDIO_USB_CONNECT_WAIT_TIMEOUT_MS 3000)
endif()

# Initialize the SDK
pico_sdk_init()

include_directories(${PICO_DISPLAY_EXAMPLES_PATH}/include)

add_subdirectory(lib/lvgl)
add_subdirectory(lib/pio_spi_tx)

include(example_auto_set_url.cmake)

function(add_subdirectory_exclude_platforms NAME)
    if (ARGN)
        if (PICO_PLATFORM IN_LIST ARGN)
            message("Skipping ${NAME} example which is unsupported on this platform")
            return()
        endif()
        foreach(PATTERN IN LISTS ARGN)
            string(REGEX MATCH "${PATTERN}" MATCHED "${PICO_PLATFORM}")
            if (MATCHED)
                message("Skipping ${NAME} example which is unsupported on this platform")
                return()
            endif()
        endforeach()
    endif()
    add_subdirectory(${NAME})
endfunction()

# Add blink example
add_subdirectory_exclude_platforms(blink)

add_compile_options(-Wall
        -Wno-format          # int != int32_t as far as the compiler is concerned because gcc has int32_t as long int
        -Wno-unused-function # we have some for the docs that aren't called
        )
if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
    add_compile_options(-Wno-maybe-uninitialized)
endif()

add_subdirectory(md144_spi_v04)