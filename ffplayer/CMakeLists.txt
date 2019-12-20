cmake_minimum_required(VERSION 2.6)
project(ffplayer)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "-D__STDC_CONSTANT_MACROS")

set (FFMPEG_DLLS
  ${FFMPEG_INSTALL_DIR}/bin/avcodec-58.dll
  ${FFMPEG_INSTALL_DIR}/bin/avformat-58.dll
  ${FFMPEG_INSTALL_DIR}/bin/avutil-56.dll
  ${FFMPEG_INSTALL_DIR}/bin/swresample-3.dll
  ${FFMPEG_INSTALL_DIR}/bin/swscale-5.dll
)
file (COPY ${FFMPEG_DLLS} DESTINATION ${PROJECT_BINARY_DIR}/Debug)
file (COPY ${FFMPEG_DLLS} DESTINATION ${PROJECT_BINARY_DIR}/Release)

add_executable(ffplayer main.cpp)
target_include_directories(ffplayer PRIVATE ${FFMPEG_INCLUDES})
target_link_libraries(ffplayer PRIVATE ${FFMPEG_LIBRARIES})
