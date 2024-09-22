cmake_minimum_required(VERSION 3.18)
project(my-bela-project VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 17)

# misc settings
set(CMAKE_SYSROOT /sysroot)
set(CMAKE_TRY_COMPILE_TARGET_TYPE "STATIC_LIBRARY")

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3 -g -fPIC -ftree-vectorize -ffast-math -pthread -D_GNU_SOURCE -D_REENTRANT -fasynchronous-unwind-tables -D__COBALT__ -D__COBALT_WRAP_")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -g -fPIC -ftree-vectorize -ffast-math -pthread -D_GNU_SOURCE -D_REENTRANT -fasynchronous-unwind-tables -D__COBALT__ -D__COBALT_WRAP_")

find_library(COBALT_LIB REQUIRED
    NAMES cobalt libcobalt
    HINTS "${CMAKE_SYSROOT}/usr/xenomai/lib"
)

find_library(NEON_LIB REQUIRED
    NAMES NE10 libNE10
    HINTS "${CMAKE_SYSROOT}/usr/lib"
)

find_library(MATHNEON_LIB REQUIRED
    NAMES mathneon libmathneon
    HINTS "${CMAKE_SYSROOT}/usr/lib"
)

include_directories(
    ${CMAKE_SYSROOT}/usr/include
    ${CMAKE_SYSROOT}/usr/include/c++/6.3.0
    ${CMAKE_SYSROOT}/usr/include/arm-linux-gnueabihf
    ${CMAKE_SYSROOT}/usr/include/arm-linux-gnueabihf/c++/6.3.0
    ./Bela
    ./Bela/include
    ${CMAKE_SYSROOT}/usr/xenomai/include
    ${CMAKE_SYSROOT}/usr/xenomai/include/cobalt
)

link_directories(
    ${CMAKE_SYSROOT}/usr/lib/gcc/arm-linux-gnueabihf/6.3.0
    ${CMAKE_SYSROOT}/usr/local/lib
    ${CMAKE_SYSROOT}/usr/xenomai/lib
    ${CMAKE_SYSROOT}/root/Bela/lib
)

link_libraries(
    cobalt modechk pthread rt
    prussdrv asound seasocks NE10 mathneon dl
    pd atomic
)

set(BELA_LINK_FLAGS "${BELA_LINK_FLAGS} \
    -B${CMAKE_SYSROOT}/usr/lib/gcc/arm-linux-gnueabihf/6.3.0 \
    -Wl,--no-as-needed -Wl,@${CMAKE_SYSROOT}/usr/xenomai/lib/cobalt.wrappers -Wl,@${CMAKE_SYSROOT}/usr/xenomai/lib/modechk.wrappers -Wl,--dynamic-list=${CMAKE_SYSROOT}/usr/xenomai/lib/dynlist.ld"
)
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${BELA_LINK_FLAGS}")
set(CMAKE_MODULE_LINKER_FLAGS "${CMAKE_MODULE_LINKER_FLAGS} ${BELA_LINK_FLAGS}")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} ${BELA_LINK_FLAGS}")

add_subdirectory(src)