# JNI是什么

JNI （Java Native Interface）java本地接口，作用是为了java代码和其他代码进行交互。这里的其他语言主要是指C/C++语言。

先来看一下一个简单的CMakeList.txt的每一行的意义, 已经标注在注释上了 

```cmake
# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html
# 关于Android Studio中使用CMake更多的信息 请参考这个网站文档

# Sets the minimum version of CMake required to build the native library.
# 设置Cmake最小版本
cmake_minimum_required(VERSION 3.22.1)

# Declares and names the project.
# 声明项目的名字 项目的名字不是目标名这个要搞清楚
project("lsso")


# 保存路径下所有.cpp结尾的文件到变量 CPP_FILES中， 用法是${CPP_FILES中}
file(GLOB CPP_FILES *.cpp)


# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.
# 创建一个命名库 可以是静态库或动态库 需要提供源码的相对路径
# 你可以顶一个多个库 CMake都会构建它们
# Gradle会自动打包动态库到你的apk中
# 参数一 库名称  这里是lsso （注意最终文件名不是它，文件名和第二个参数有关）
# 参数二 库类型 一般是SHARED（动态库也叫共享库） 或 STATIC（静态库）
#       如果是共享库 最终文件名 liblsso.so  静态库 liblsso.a
# 参数三  多个参数源代码 可以是上面的变量 也可以是所有源文件的相对路径
add_library( # Sets the name of the library.
        lsso

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        ${CPP_FILES}
        LogUtils.h)

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.
# 搜索一个预构建的库 保存它的路径到变量里
# 这里是搜索的是NDK的日志库 它的真实路径
# 也就是log-lib这个变量的值 = ${SDK路径}/${NDK路径}/liblog.so
find_library( # Sets the name of the path variable.
        log-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log)




# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.
# 指定要链接到你的目标库的的库（们 多个）
# 可以是你构建的库 三方库 或系统库
target_link_libraries( # Specifies the target library.
        lsso

        # Links the target library to the log library
        # included in the NDK.
        ${log-lib})
```

