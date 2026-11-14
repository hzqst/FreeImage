# ZLib静态库
set(ZLIB_SOURCES
    adler32.c
    compress.c
    crc32.c
    deflate.c
    gzclose.c
    gzlib.c
    gzread.c
    gzwrite.c
    infback.c
    inffast.c
    inflate.c
    inftrees.c
    trees.c
    uncompr.c
    zutil.c
)

add_library(ZLib STATIC ${ZLIB_SOURCES})

# 非 Windows 下 gz*.c 调用 read/write/close/lseek，需要 zconf.h 包含 unistd.h；
# z_off_t 也随之变为 off_t，因此对使用者同样可见
if(NOT WIN32)
    target_compile_definitions(ZLib PUBLIC HAVE_UNISTD_H)
endif()

target_include_directories(ZLib PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include/zlib>
)

# 设置输出名称
set_target_properties(ZLib PROPERTIES
    OUTPUT_NAME "zlib"
    DEBUG_POSTFIX "d"
) 