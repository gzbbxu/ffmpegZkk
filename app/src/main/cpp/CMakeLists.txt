# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.10.2)

# Declares and names the project.

project("myffmpeg")
link_directories("${CMAKE_SOURCE_DIR}/../jniLibs")
#设置生成的so动态库最后输出的路径 方式1
set(jnilibs "${CMAKE_SOURCE_DIR}/../jniLibs")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${jnilibs}/${ANDROID_ABI})
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall ")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -fexceptions -DXLOCALE_NOT_USED=1 -DNULL=0 -DNO_SSTREAM=1 -DSOCKLEN_T=socklen_t -UIP_ADD_SOURCE_MEMBERSHIP ") # -fno-rtti
#set(ANDROID_STL "c++_shared")

include_directories(${CMAKE_SOURCE_DIR}/ffmpeg/include)
include_directories(${CMAKE_SOURCE_DIR}/fdk_aac/include)
include_directories(${CMAKE_SOURCE_DIR}/x264/include)
include_directories(${CMAKE_SOURCE_DIR})



# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

add_library( # Sets the name of the library.
        myffmpeg

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        native-lib.cpp)

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
        log-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.
add_library( fdk-aac
        STATIC
        IMPORTED )
set_target_properties( fdk-aac
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libfdk-aac.a )

add_library( x264
        STATIC
        IMPORTED )
set_target_properties( x264
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libx264.a )

add_library( avcodec
        STATIC
        IMPORTED )
set_target_properties( avcodec
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libavcodec.a )


add_library( avdevice
        STATIC
        IMPORTED )
set_target_properties( avdevice
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libavdevice.a )


add_library( avfilter
        STATIC
        IMPORTED )
set_target_properties( avfilter
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libavfilter.a )



add_library( avformat
        STATIC
        IMPORTED )
set_target_properties( avformat
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libavformat.a )



add_library( avresample
        STATIC
        IMPORTED )
set_target_properties( avresample
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libavresample.a )

add_library( avutil
        STATIC
        IMPORTED )
set_target_properties( avutil
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libavutil.a )



add_library( postproc
        STATIC
        IMPORTED )
set_target_properties( postproc
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libpostproc.a )


add_library( swresample
        STATIC
        IMPORTED )
set_target_properties( swresample
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libswresample.a )




add_library(  swscale
        STATIC
        IMPORTED )
set_target_properties( swscale
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libswscale.a )

add_library( opencv_java4
        STATIC
        IMPORTED )
set_target_properties( opencv_java4
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libopencv_java4.so)

target_link_libraries( # Specifies the target library.
        myffmpeg

        android
        z
        c
        m
        dl
        log
        jnigraphics
        mediandk
        opencv_java4
        avformat
        avdevice
        avfilter
        avcodec
        avutil
        postproc
        swresample
        avresample
        swscale
        x264
        fdk-aac

        # Links the target library to the log library
        # included in the NDK.
        ${log-lib})