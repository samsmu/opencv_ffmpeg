cmake_minimum_required(VERSION 2.6)
project(opencv_ffmpeg)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "-D__STDC_CONSTANT_MACROS")

set( HEADERS
    cap_ffmpeg_api.hpp
    cap_ffmpeg_impl.hpp
    ffmpeg_codecs.hpp
)

set( SOURCE
  opencv_ffmpeg.cpp
)

add_library(opencv_ffmpeg SHARED ${SOURCE} ${HEADERS})
target_include_directories(opencv_ffmpeg PRIVATE ${FFMPEG_INCLUDES})
target_link_libraries(opencv_ffmpeg PRIVATE ${FFMPEG_LIBRARIES_STATIC})