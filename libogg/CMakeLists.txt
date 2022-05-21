cmake_minimum_required(VERSION 3.0.0)
project(ogg VERSION 0.1.0)

include(CTest)
enable_testing()

add_compile_definitions(__ESP32__)

set(SRC src)


set(OGG_SOURCES
    ${SRC}/bitwise.c
    ${SRC}/framing.c
)

set(SIZE16 int16_t)
set(USIZE16 uint16_t)
set(SIZE32 int32_t)
set(USIZE32 uint32_t)
set(SIZE64 int64_t)
set(USIZE64 uint64_t)

set(INCLUDE_INTTYPES_H 0)
set(INCLUDE_STDINT_H 1)
set(INCLUDE_SYS_TYPES_H 0)

configure_file(include/ogg/config_types.h.in include/ogg/config_types.h @ONLY)

add_library(ogg ${OGG_SOURCES})

target_include_directories(ogg PUBLIC include)


set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
