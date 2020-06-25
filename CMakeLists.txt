cmake_minimum_required(VERSION 2.6)

find_path(AVCODEC_INCLUDE_DIR libavcodec/avcodec.h ${FFMPEG_INSTALL_DIR}/include)
find_library(AVCODEC_LIBRARY avcodec ${FFMPEG_INSTALL_DIR}/bin)
file (COPY ${FFMPEG_INSTALL_DIR}/lib/libavcodec.a DESTINATION ${PROJECT_BINARY_DIR}/ffmpeg-build)
file (RENAME ${PROJECT_BINARY_DIR}/ffmpeg-build/libavcodec.a ${PROJECT_BINARY_DIR}/ffmpeg-build/avcodec.lib)
find_library(AVCODEC_LIBRARY_STATIC avcodec ${PROJECT_BINARY_DIR}/ffmpeg-build)

find_path(AVFORMAT_INCLUDE_DIR libavformat/avformat.h ${FFMPEG_INSTALL_DIR}/include)
find_library(AVFORMAT_LIBRARY avformat ${FFMPEG_INSTALL_DIR}/bin)
file (COPY ${FFMPEG_INSTALL_DIR}/lib/libavformat.a DESTINATION ${PROJECT_BINARY_DIR}/ffmpeg-build)
file (RENAME ${PROJECT_BINARY_DIR}/ffmpeg-build/libavformat.a ${PROJECT_BINARY_DIR}/ffmpeg-build/avformat.lib)
find_library(AVFORMAT_LIBRARY_STATIC avformat ${PROJECT_BINARY_DIR}/ffmpeg-build)

find_path(AVUTIL_INCLUDE_DIR libavutil/avutil.h ${FFMPEG_INSTALL_DIR}/include)
find_library(AVUTIL_LIBRARY avutil ${FFMPEG_INSTALL_DIR}/bin)
file (COPY ${FFMPEG_INSTALL_DIR}/lib/libavutil.a DESTINATION ${PROJECT_BINARY_DIR}/ffmpeg-build)
file (RENAME ${PROJECT_BINARY_DIR}/ffmpeg-build/libavutil.a ${PROJECT_BINARY_DIR}/ffmpeg-build/avutil.lib)
find_library(AVUTIL_LIBRARY_STATIC avutil ${PROJECT_BINARY_DIR}/ffmpeg-build)

find_path(AVDEVICE_INCLUDE_DIR libavdevice/avdevice.h ${FFMPEG_INSTALL_DIR}/include)
find_library(AVDEVICE_LIBRARY avdevice ${FFMPEG_INSTALL_DIR}/bin)
file (COPY ${FFMPEG_INSTALL_DIR}/lib/libavdevice.a DESTINATION ${PROJECT_BINARY_DIR}/ffmpeg-build)
file (RENAME ${PROJECT_BINARY_DIR}/ffmpeg-build/libavdevice.a ${PROJECT_BINARY_DIR}/ffmpeg-build/avdevice.lib)
find_library(AVDEVICE_LIBRARY_STATIC avdevice ${PROJECT_BINARY_DIR}/ffmpeg-build)

find_path(SWSCALE_INCLUDE_DIR libswscale/swscale.h ${FFMPEG_INSTALL_DIR}/include)
find_library(SWSCALE_LIBRARY swscale ${FFMPEG_INSTALL_DIR}/bin)
file (COPY ${FFMPEG_INSTALL_DIR}/lib/libswscale.a DESTINATION ${PROJECT_BINARY_DIR}/ffmpeg-build)
file (RENAME ${PROJECT_BINARY_DIR}/ffmpeg-build/libswscale.a ${PROJECT_BINARY_DIR}/ffmpeg-build/swscale.lib)
find_library(SWSCALE_LIBRARY_STATIC swscale ${PROJECT_BINARY_DIR}/ffmpeg-build)

file (COPY ${FFMPEG_INSTALL_DIR}/lib/libswresample.a DESTINATION ${PROJECT_BINARY_DIR}/ffmpeg-build)
file (RENAME ${PROJECT_BINARY_DIR}/ffmpeg-build/libswresample.a ${PROJECT_BINARY_DIR}/ffmpeg-build/swresample.lib)
find_library(AVRESAMPLE_LIBRARY_STATIC swresample ${PROJECT_BINARY_DIR}/ffmpeg-build)

find_library(X264_LIBRARY_STATIC libx264 ${X264_DIR})

set (FFMPEG_INCLUDES 
  ${AVCODEC_INCLUDE_DIR}
  ${AVFORMAT_INCLUDE_DIR}
  ${AVUTIL_INCLUDE_DIR}
  ${AVDEVICE_INCLUDE_DIR}
  ${SWSCALE_INCLUDE_DIR}
)

set (FFMPEG_LIBRARIES
  ${AVCODEC_LIBRARY}
  ${AVFORMAT_LIBRARY}
  ${AVUTIL_LIBRARY}
  ${AVDEVICE_LIBRARY}
  ${SWSCALE_LIBRARY} 
)

set (FFMPEG_LIBRARIES_STATIC
  ${AVCODEC_LIBRARY_STATIC}
  ${AVFORMAT_LIBRARY_STATIC}
  ${AVUTIL_LIBRARY_STATIC}
  ${AVDEVICE_LIBRARY_STATIC}
  ${SWSCALE_LIBRARY_STATIC} 
  ${AVRESAMPLE_LIBRARY_STATIC}
  ${X264_LIBRARY_STATIC}
)
add_subdirectory( ffplayer )
add_subdirectory( opencv_ffmpeg )
