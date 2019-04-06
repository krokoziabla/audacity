#directory cmake-proxies/portmixer
set(TARGET portmixer)
set(TARGET_SOURCE ${LIB_SRC_DIRECTORY}${TARGET})
project(${TARGET})

include(CheckIncludeFiles)

#check_include_files("alsa/asoundlib.h pa_linux_alsa.h" HAVE_ALSA)

add_library(${TARGET} STATIC
    "${TARGET_SOURCE}/src/px_mixer.c"
    "$<$<BOOL:${WIN32}>:${TARGET_SOURCE}/src/px_win_common.c>"
    "$<$<BOOL:${WIN32}>:${TARGET_SOURCE}/src/px_win_ds.c>"
    "$<$<BOOL:${WIN32}>:${TARGET_SOURCE}/src/px_win_endpoint.c>"
    "$<$<BOOL:${WIN32}>:${TARGET_SOURCE}/src/px_win_wasapi.c>"
    "$<$<BOOL:${WIN32}>:${TARGET_SOURCE}/src/px_win_wmme.c>")

target_compile_definitions(${TARGET} PRIVATE
    $<$<BOOL:${WIN32}>:PX_USE_WIN_DSOUND>
    $<$<BOOL:${WIN32}>:PX_USE_WIN_MME>
    $<$<BOOL:${WIN32}>:PX_USE_WIN_WASAPI>
    $<$<BOOL:${WIN32}>:_CRT_SECURE_NO_WARNINGS>)

target_include_directories(${TARGET} PUBLIC "${TARGET_SOURCE}/include")
target_link_libraries(${TARGET} PUBLIC portaudio_static)
set_target_properties(${TARGET} PROPERTIES OSX_ARCHITECTURES "")
