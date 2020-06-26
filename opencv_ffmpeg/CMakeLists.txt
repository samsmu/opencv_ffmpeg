cmake_minimum_required(VERSION 2.6)

include(ExternalProject)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "-D__STDC_CONSTANT_MACROS")

# set cmake debug postfix to d
set(CMAKE_DEBUG_POSTFIX "d")
cmake_policy(SET CMP0020 NEW)

# INTERFACE_LINK_LIBRARIES
if( POLICY CMP0022 )
  cmake_policy( SET CMP0022 NEW )
endif()

if(COMMAND CMAKE_POLICY)
  cmake_policy(SET CMP0003 NEW)
endif()

# Suppress specific warnings
if( MSVC )
    set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4996 /wd4305 /wd4503 /wd4290 /wd4251 /wd4005" )

    set( CMAKE_EXE_LINKER_FLAGS    "${CMAKE_EXE_LINKER_FLAGS}    /ignore:4221" )
    set( CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} /ignore:4221" )
    set( CMAKE_STATIC_LINKER_FLAGS "${CMAKE_STATIC_LINKER_FLAGS} /ignore:4221" )
    set( CMAKE_MODULE_LINKER_FLAGS "${CMAKE_MODULE_LINKER_FLAGS} /ignore:4221" )
    
    add_definitions( -DNOMINMAX )
endif()

if( WIN32 )
    add_definitions( -DWIN32_LEAN_AND_MEAN )
endif()

# Debug information for Windows
if(${${MY_PROJECT_NAME}_PDB})
  set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /Zi")
  set(CMAKE_SHARED_LINKER_FLAGS_RELEASE "${CMAKE_SHARED_LINKER_FLAGS_RELEASE} /debug")
  set(CMAKE_EXE_LINKER_FLAGS_RELEASE "${CMAKE_EXE_LINKER_FLAGS_RELEASE} /debug")
  set(CMAKE_MODULE_LINKER_FLAGS_RELEASE "${CMAKE_MODULE_LINKER_FLAGS_RELEASE} /debug")

  set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /Zi")
  set(CMAKE_SHARED_LINKER_FLAGS_DEBUG "${CMAKE_SHARED_LINKER_FLAGS_DEBUG} /debug /OPT:REF")
  set(CMAKE_EXE_LINKER_FLAGS_DEBUG "${CMAKE_EXE_LINKER_FLAGS_DEBUG} /debug /OPT:REF")
  set(CMAKE_MODULE_LINKER_FLAGS_DEBUG "${CMAKE_MODULE_LINKER_FLAGS_DEBUG} /debug /OPT:REF")
endif()

add_definitions( -DTRACKING_EXPORT_CPP=__declspec\(dllexport\) )

if( CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT )
    set( CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/installed" CACHE PATH "default install path" FORCE )
endif()


set(ep_prefix "${CMAKE_BINARY_DIR}/ep")
set_property(DIRECTORY PROPERTY EP_PREFIX ${ep_prefix})

set(ep_install_dir ${ep_prefix})
set(ep_suffix "-cmake")
set(ep_build_shared_libs ON)
set(ep_build_testing OFF)

# Compute -G arg for configuring external projects with the same CMake generator:
if(CMAKE_EXTRA_GENERATOR)
  set(gen "${CMAKE_EXTRA_GENERATOR} - ${CMAKE_GENERATOR}")
else()
  set(gen "${CMAKE_GENERATOR}")
endif()

# Use this value where semi-colons are needed in ep_add args:
set(sep "^^")

##  Explicit setting of Windows version

if(WIN32)
  # for the best compatibility, always set Windows version to 7
  set(ver "0x0601")

  # this method adds the necessary compiler flag
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D_WIN32_WINNT=${ver}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D_WIN32_WINNT=${ver}")
  # this adds a preprocessor definition to the project
  add_definitions(-D_WIN32_WINNT=${ver})
endif()

##  Explicit setting of Unix version

if(CMAKE_COMPILER_IS_GNUCXX)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
endif()

##

set(ep_common_C_FLAGS "${CMAKE_C_FLAGS}")
set(ep_common_CXX_FLAGS "${CMAKE_CXX_FLAGS}")

if(MSVC_VERSION)
  set(ep_common_C_FLAGS "${ep_common_C_FLAGS} /bigobj /MP")
  set(ep_common_CXX_FLAGS "${ep_common_CXX_FLAGS} /bigobj /MP")
endif()

set(ep_common_args
  -DBUILD_TESTING:BOOL=${ep_build_testing}
  -DCMAKE_INSTALL_PREFIX:PATH=${ep_install_dir}
  -DCMAKE_BUILD_TYPE:STRING=${CMAKE_BUILD_TYPE}
  -DCMAKE_C_COMPILER:FILEPATH=${CMAKE_C_COMPILER}
  -DCMAKE_CXX_COMPILER:FILEPATH=${CMAKE_CXX_COMPILER}
  -DCMAKE_C_FLAGS:STRING=${ep_common_C_FLAGS}
  -DCMAKE_CXX_FLAGS:STRING=${ep_common_CXX_FLAGS}
  -DCMAKE_CXX_STANDARD:STRING=${CMAKE_CXX_STANDARD}
  -DCMAKE_CXX_EXTENSIONS:STRING=${CMAKE_CXX_EXTENSIONS}
  -DCMAKE_CXX_STANDARD_REQUIRED:STRING=${CMAKE_CXX_STANDARD_REQUIRED}

  -DCMAKE_CXX_FLAGS_RELEASE:STRING=${CMAKE_CXX_FLAGS_RELEASE}
  -DCMAKE_SHARED_LINKER_FLAGS_RELEASE:STRING=${CMAKE_SHARED_LINKER_FLAGS_RELEASE}
  -DCMAKE_EXE_LINKER_FLAGS_RELEASE:STRING=${CMAKE_EXE_LINKER_FLAGS_RELEASE}
  -DCMAKE_MODULE_LINKER_FLAGS_RELEASE:STRING=${CMAKE_MODULE_LINKER_FLAGS_RELEASE}
)

ExternalProject_Add(OpenCV
    GIT_REPOSITORY "git@github.com:opencv/opencv.git"
    GIT_TAG "4.3.0"
    PREFIX opencv-cmake
    SOURCE_DIR ${ep_prefix}/OpenCV-src
    BINARY_DIR ${ep_prefix}/OpenCV-build
    INSTALL_DIR ${ep_install_dir}
    CMAKE_ARGS
      ${ep_common_args}
    -DCMAKE_INSTALL_PREFIX:PATH=${ep_install_dir}
    -DBUILD_TESTS:BOOL=OFF
    -DBUILD_PERF_TESTS:BOOL=OFF
    -DBUILD_WITH_DEBUG_INFO:BOOL=${MY_PROJECT_NAME}_PDB
)

project(opencv_ffmpeg)

set( SOURCE
  opencv_ffmpeg.cpp
)
add_definitions( -DNOMINMAX -DBUILD_PLUGIN -DHAVE_FFMPEG )

include_directories( ${ep_prefix}/include )
include_directories( ${ep_prefix}/OpenCV-src/modules/core/include/ )
include_directories( ${ep_prefix}/OpenCV-src/modules/videoio/src/ )

link_directories( ${ep_prefix}/x64/vc14/lib/ )

add_library(opencv_ffmpeg SHARED ${SOURCE} )
add_dependencies(opencv_ffmpeg OpenCV)
target_include_directories(opencv_ffmpeg PRIVATE ${FFMPEG_INCLUDES})
target_link_libraries(opencv_ffmpeg PRIVATE ${OpenCV_LIBS} ${FFMPEG_LIBRARIES_STATIC})