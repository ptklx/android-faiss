自己简单配置下
用extra，faiss就可编译64位的
cmake_minimum_required(VERSION 3.10.1)
add_definitions(-w)
set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE}   -s  -O3 -DSkip_f2c_Undefs -DNO_LONG_LONG -DNO_BLAS_WRAP")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -s  -O3 -DSkip_f2c_Undefs -DNO_LONG_LONG -DNO_BLAS_WRAP")
add_definitions(-D ANDROID_BIT64)
file(GLOB_RECURSE SRC_FILES  *)
source_group( "src"       FILES ${SRC_FILES} )

add_definitions(-D_USE_MATH_DEFINES)
message(STATUS "cartFaiss SRC_FILES = ${SRC_FILES}")
add_library(cartFaiss
    STATIC
    ${SRC_FILES}
        )
set(SYS_LIBS
    "log"
    "z"
   )
target_link_libraries( cartFaiss
        cartUtility
        ${SYS_LIBS}
        )




######end#######
# android-faiss
facebook faiss for android

ndk: android-ndk-r19c

lapack: version 3.4.2

1.移除了未使用到的文件

faiss: version 1.5.3

1. 移除了未使用的文件夹

2. 重定义 FINTEGER

3. 在FaissAssert.h 添加 android log.h 方便调试

4. 64-bit(arm64-v8a   x86-64) 是可以正常使用的  只要你的设备内存足够大

5. 关于32位库警告:

faiss主要还是基于64位设计,32位并不能正常运行;

更改了部分源码使它能在32位(armeabi-v7a  x86)上运行 但并不保证运行一定稳定。

相关更改可以比对faiss文件夹和faiss-32文件夹的不同

相关更改:
        
        a.  replace size_t with int64_t 
        
        b.  replace long  with int64_t
        
        c.  replace "1L << 40" with "1LL << 40" ("1L << 40" will overflow in 32-bit because of long is 32 bits);
        
            the same as "1L << 60";
            

6. x86 x86_64 尚未经过测试


相关源码参考:

https://github.com/facebookresearch/faiss

https://github.com/weilaudm/nxgbcc-android-faiss
