
set(CMAKE_C_COMPILER "C:/Espressif/tools/xtensa-esp32-elf/esp-2021r2-patch2-8.4.0/xtensa-esp32-elf/bin/xtensa-esp32-elf-gcc.exe")
set(CMAKE_CXX_COMPILER "C:/Espressif/tools/xtensa-esp32-elf/esp-2021r2-patch2-8.4.0/xtensa-esp32-elf/bin/xtensa-esp32-elf-g++.exe")
set(CMAKE_TRY_COMPILE_TARGET_TYPE "STATIC_LIBRARY")
add_compile_options(-mlongcalls)

cmake_minimum_required(VERSION 3.0.0)
project(vorbis VERSION 0.1.0)

#include(CTest)
#enable_testing()


add_subdirectory(libogg)

include_directories(include)
include_directories(lib)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/libogg/include)

set(SRC lib)

set(VORBIS_PUBLIC_HEADERS
    include/vorbis/codec.h
    include/vorbis/vorbisenc.h
    include/vorbis/vorbisfile.h
)

set(VORBIS_HEADERS
    ${SRC}/envelope.h
    ${SRC}/lpc.h
    ${SRC}/lsp.h
    ${SRC}/codebook.h
    ${SRC}/misc.h
    ${SRC}/psy.h
    ${SRC}/masking.h
    ${SRC}/os.h
    ${SRC}/mdct.h
    ${SRC}/smallft.h
    ${SRC}/highlevel.h
    ${SRC}/registry.h
    ${SRC}/scales.h
    ${SRC}/window.h
    ${SRC}/lookup.h
    ${SRC}/lookup_data.h
    ${SRC}/codec_internal.h
    ${SRC}/backends.h
    ${SRC}/bitrate.h
)

set(VORBIS_SOURCES
    ${SRC}/mdct.c
    ${SRC}/smallft.c
    ${SRC}/block.c
    ${SRC}/envelope.c
    ${SRC}/window.c
    ${SRC}/lsp.c
    ${SRC}/lpc.c
    ${SRC}/analysis.c
    ${SRC}/synthesis.c
    ${SRC}/psy.c
    ${SRC}/info.c
    ${SRC}/floor1.c
    ${SRC}/floor0.c
    ${SRC}/res0.c
    ${SRC}/mapping0.c
    ${SRC}/registry.c
    ${SRC}/codebook.c
    ${SRC}/sharedbook.c
    ${SRC}/lookup.c
    ${SRC}/bitrate.c
)

set(VORBISFILE_SOURCES
    ${SRC}/vorbisfile.c
)

set(VORBISENC_SOURCES
    ${SRC}/vorbisenc.c
)

add_library(vorbis_pre ${VORBIS_HEADERS} ${VORBIS_SOURCES})
add_library(vorbisenc ${VORBISENC_SOURCES})
add_library(vorbisfile ${VORBISFILE_SOURCES})

target_link_libraries(vorbis_pre PUBLIC ogg)
target_link_libraries(vorbisenc PUBLIC ogg)
target_link_libraries(vorbisfile PUBLIC ogg)


set(C_LIB ${CMAKE_BINARY_DIR}/libvorbis.a)

message(ERROR "$<TARGET_FILE:ogg>")

add_custom_target(combined
        COMMAND ar -x $<TARGET_FILE:vorbis_pre>
        COMMAND ar -x $<TARGET_FILE:ogg>
        COMMAND ar -qcs ${C_LIB} *.o
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
        DEPENDS vorbis_pre ogg
        )

add_library(vorbis STATIC IMPORTED GLOBAL)
add_dependencies(vorbis combined)

#add_library(asd INTERFACE)
#target_link_libraries(asd INTERFACE vorbis_pre ogg)



##set(CPACK_PROJECT_NAME ${PROJECT_NAME})
#set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
#include(CPack)
