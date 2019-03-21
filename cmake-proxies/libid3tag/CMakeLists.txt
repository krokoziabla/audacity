#directory cmake-proxies/libid3tag
set( TARGET libid3tag )
set( TARGET_SOURCE ${LIB_SRC_DIRECTORY}${TARGET} )
project( ${TARGET} )

set( SOURCES
${LIB_SRC_DIRECTORY}libid3tag/compat.c
${LIB_SRC_DIRECTORY}libid3tag/crc.c
${LIB_SRC_DIRECTORY}libid3tag/debug.c
${LIB_SRC_DIRECTORY}libid3tag/field.c
${LIB_SRC_DIRECTORY}libid3tag/file.c
${LIB_SRC_DIRECTORY}libid3tag/frame.c
${LIB_SRC_DIRECTORY}libid3tag/frametype.c
${LIB_SRC_DIRECTORY}libid3tag/genre.c
${LIB_SRC_DIRECTORY}libid3tag/latin1.c
${LIB_SRC_DIRECTORY}libid3tag/parse.c
${LIB_SRC_DIRECTORY}libid3tag/render.c
${LIB_SRC_DIRECTORY}libid3tag/tag.c
${LIB_SRC_DIRECTORY}libid3tag/ucs4.c
${LIB_SRC_DIRECTORY}libid3tag/utf16.c
${LIB_SRC_DIRECTORY}libid3tag/utf8.c
${LIB_SRC_DIRECTORY}libid3tag/util.c
${LIB_SRC_DIRECTORY}libid3tag/version.c
)
add_library( ${TARGET} STATIC ${SOURCES})

find_package(ZLIB)
if(ZLIB_FOUND)
    include(ExternalProject)
    ExternalProject_Add(zlib
        URL "http://www.zlib.net/zlib-1.2.11.tar.xz"
        CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/zlib-prefix)
    add_dependencies(${TARGET} zlib)
#   find_package(ZLIB REQUIRED PATHS "${CMAKE_CURRENT_BINARY_DIR}/zlib-prefix")
#   target_include_directories(${TARGET} PRIVATE ${ZLIB_INCLUDE_DIRS})
#   find_library(ZLIB_STATIC_LIB libz.a)
#   target_link_libraries(${TARGET} PUBLIC ${ZLIB_STATIC_LIB})
else()
    target_link_libraries(${TARGET} PUBLIC ZLIB::ZLIB)
endif()
