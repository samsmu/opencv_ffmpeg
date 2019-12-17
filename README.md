Contains two applications:


ffplayer - Small example that dynamically links with ffmpeg and saves frames after 10th into .ppm files. Allows to check if ffmpeg is working and also allows to debug it with VS.

ffmpeg_opencv - OpenCV interface to ffmpeg, statically linked with ffmpeg libraries. Rename it to opencv_ffmpeg_64.dll and replace appropriate DLL in OpenCV in order to make it work.
OpenCV loads it in runtime, so no need to re-compile anything.


In order to build this project, you need:

1. Build ffmpeg and install it. Follow instructions from here: https://gist.github.com/RangelReale/3e6392289d8ba1a52b6e70cdd7e10282
and use next command to build both static and dynamic ffmpeg libraries:

./configure --toolchain=msvc --arch=x86_64 --enable-yasm  --enable-asm --enable-shared --disable-programs --enable-avresample --enable-libx264 --enable-gpl --prefix=./install

Be carefull, sometimes ./configure stalls in the middle and you need to kill perl.exe process to make it continue.


2. Manually set up path to ffmpeg install dir in base CMakeLists.txt:

set (FFMPEG_INSTALL_DIR "%YOUR_PATH_TO_FFMPEG%/FFmpeg/install")
