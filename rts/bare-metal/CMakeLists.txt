cmake_minimum_required(VERSION 3.7.2)

project(libsel4-idris-rts C)

file(GLOB static
    rts/idris_rts.c
    rts/idris_heap.c
    rts/idris_gc.c
    rts/idris_gmp.c
    rts/idris_bitstring.c
    rts/idris_stats.c
    rts/idris_utf8.c
    rts/mini-gmp.c
    rts/seL4/idris_main.c
)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -W -Wall")
# Ignore some warnings
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-unused-parameter")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-sign-compare")
# Don't use the host machines' environment when cross compiling
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DIDRIS_TARGET_OS=\"\\\"bare-metal\\\"\"")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DBARE_METAL")
execute_process(
    COMMAND ${CMAKE_C_COMPILER} -dumpmachine
    OUTPUT_VARIABLE MACHINE
    OUTPUT_STRIP_TRAILING_WHITESPACE
)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DIDRIS_TARGET_TRIPLE=\"\\\"${MACHINE}\\\"\"")

add_library(idris-rts-bare-metal EXCLUDE_FROM_ALL ${static})
target_include_directories(idris-rts-bare-metal PUBLIC rts)