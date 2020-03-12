Contains two applications:


ffplayer - Small example that dynamically links with ffmpeg and saves frames after 10th into .ppm files. Allows to check if ffmpeg is working and also allows to debug it with VS.

ffmpeg_opencv - OpenCV interface to ffmpeg, statically linked with ffmpeg libraries. Rename it to opencv_ffmpeg410_64.dll and replace appropriate DLL in OpenCV in order to make it work. For Autoplan place it in WebDAV:\autoplan_downloads\autoplan_ar\depends_bins\vc14_x64\opencv.

OpenCV loads it in runtime, so no need to re-compile anything.


In order to build this project, you need:

1. Build ffmpeg and install it. Follow instructions from here: https://gist.github.com/RangelReale/3e6392289d8ba1a52b6e70cdd7e10282
and use next tow commands to build both static and dynamic ffmpeg libraries:

  - ./configure --toolchain=msvc --arch=x86_64 --enable-yasm  --enable-asm --enable-shared --disable-programs --enable-avresample --enable-libx264 --enable-gpl --prefix=./install && make && make install
  - ./configure --toolchain=msvc --arch=x86_64 --enable-yasm  --enable-asm --disable-programs --enable-avresample --enable-libx264 --enable-gpl --prefix=./install && make && make install

During this step, perl.exe may be stall. In this case, kill it manually after some time passes.

2. Manually set up paths to ffmpeg and x264 install dir in base CMakeLists.txt:
  - set (FFMPEG_INSTALL_DIR "%YOUR_PATH_TO_FFMPEG%/FFmpeg/install")
  - set (X264_DIR "%YOUR_PATH_TO_X264%")
