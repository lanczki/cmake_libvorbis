cmake_minimum_required(VERSION 3.0.0)
project(vorbis VERSION 0.1.0)

include(CTest)
enable_testing()

add_compile_definitions(INCLUDE_STDINT_H=1)

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

add_library(vorbis ${VORBIS_HEADERS} ${VORBIS_SOURCES})
add_library(vorbisenc ${VORBISENC_SOURCES})
add_library(vorbisfile ${VORBISFILE_SOURCES})

target_link_libraries(vorbis PUBLIC ogg)
target_link_libraries(vorbisenc PUBLIC ogg)
target_link_libraries(vorbisfile PUBLIC ogg)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
