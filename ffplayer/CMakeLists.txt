cmake_minimum_required(VERSION 2.6)
project(ffplayer)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "-D__STDC_CONSTANT_MACROS")

find_path(AVCODEC_INCLUDE_DIR libavcodec/avcodec.h D:/ffmpeg/FFmpeg/install/include)
find_library(AVCODEC_LIBRARY avcodec D:/ffmpeg/FFmpeg/install/bin)

find_path(AVFORMAT_INCLUDE_DIR libavformat/avformat.h D:/ffmpeg/FFmpeg/install/include)
find_library(AVFORMAT_LIBRARY avformat D:/ffmpeg/FFmpeg/install/bin)

find_path(AVUTIL_INCLUDE_DIR libavutil/avutil.h D:/ffmpeg/FFmpeg/install/include)
find_library(AVUTIL_LIBRARY avutil D:/ffmpeg/FFmpeg/install/bin)

find_path(AVDEVICE_INCLUDE_DIR libavdevice/avdevice.h D:/ffmpeg/FFmpeg/install/include)
find_library(AVDEVICE_LIBRARY avdevice D:/ffmpeg/FFmpeg/install/bin)

find_path(SWSCALE_INCLUDE_DIR libswscale/swscale.h D:/ffmpeg/FFmpeg/install/include)
find_library(SWSCALE_LIBRARY swscale D:/ffmpeg/FFmpeg/install/bin)

add_executable(ffplayer main.cpp)
target_include_directories(ffplayer PRIVATE ${AVCODEC_INCLUDE_DIR} ${AVFORMAT_INCLUDE_DIR} ${AVUTIL_INCLUDE_DIR} ${AVDEVICE_INCLUDE_DIR} ${SWSCALE_INCLUDE_DIR})
target_link_libraries(ffplayer PRIVATE ${AVCODEC_LIBRARY} ${AVFORMAT_LIBRARY} ${AVUTIL_LIBRARY} ${AVDEVICE_LIBRARY} ${SWSCALE_LIBRARY})