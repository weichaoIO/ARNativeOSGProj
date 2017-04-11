# CMake版本信息
cmake_minimum_required(VERSION 3.4.1)

# 支持-std=gnu++11
set(CMAKE_VERBOSE_MAKEFILE on)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")

# 工程路径
set(pathToProject D:/android_studio_workspace/ARNativeOSGProj)
# Native 路径
set(pathToNative ${pathToProject}/aRNativeOSG/src/main/cpp)

# 配置加载native依赖
include_directories(${pathToProject}/ARToolKit)
include_directories(${pathToProject}/ARToolKit/android)

# 添加待编译的文件
add_library(ARNativeOSG SHARED ${pathToNative}/ARMarkerSquare.c
                               ${pathToNative}/ARMarkerSquare.h
                               ${pathToNative}/ARNativeOSG.cpp
                               ${pathToNative}/color_convert_common.c
                               ${pathToNative}/color_convert_common.h
                               ${pathToNative}/osgPlugins.h
                               ${pathToNative}/VirtualEnvironment.c
                               ${pathToNative}/VirtualEnvironment.h)

# 动态方式加载
add_library(so_libc++_shared SHARED IMPORTED)
# 引入so文件
set_target_properties(so_libc++_shared PROPERTIES IMPORTED_LOCATION ${pathToProject}/aRNativeOSG/src/main/jniLibs/${ANDROID_ABI}/libc++_shared.so)

# 动态方式加载
add_library(so_libARNativeOSGNative SHARED IMPORTED)
# 引入so文件
set_target_properties(so_libARNativeOSGNative PROPERTIES IMPORTED_LOCATION ${pathToProject}/aRNativeOSG/src/main/jniLibs/${ANDROID_ABI}/libARNativeOSGNative.so)

target_link_libraries(ARNativeOSG
                      so_libc++_shared so_libARNativeOSGNative
                      log GLESv1_CM z)