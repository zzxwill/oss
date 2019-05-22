# Installation {#concept_nrg_gym_4gb .concept}

This topic describes how to install the OSS C++ SDK.

## Prerequisites {#section_g2p_w14_4gb .section}

Install a compiler that supports C++ V11 and later.

-   Visual Studio V2013 and later
-   GCC V4.8 and later
-   Clang V3.3 and later

## Download the SDK {#section_s2t_wn4_4gb .section}

[Download the SDK directly](https://github.com/aliyun/aliyun-oss-cpp-sdk/archive/master.zip)

[Download from GitHub](https://github.com/aliyun/aliyun-oss-cpp-sdk.git)

## Install the SDK {#section_vlq_f44_4gb .section}

You can install the SDK in Linux, Windows, Android, and Mac.

-   Linux:

    1.  Install CMake.

        Install CMake V3.1 and later. Open the SDK installation package and use CMake to compile and generate the required files.

        The compilation commands are as follows:

        ```
        cd <path/to/aliyun-oss-cpp-sdk>
        mkdir build
        cd build
        cmake ..
        ```

    2.  Install third-party libraries libcurl and openssl.

        RedHat/Centos:

        ```
        yum –y install libcurl-devel openssl-devel
        ```

        Fedora:

        ```
        sudo dnf install libcurl-devel openssl-devel
        ```

    3.  Install the SDK.

        ```
        make && make install
        ```

    **Note:** RTTI is disabled in the C++ SDK by default. Therefore, add -std=c++11, -fno-rtti, and -lalibabacloud-oss-cpp-sdk when you use g++ to compile and run the SDK. Example:

    ```
    g++ test.cpp -std=c++11 -fno-rtti -lalibabacloud-oss-cpp-sdk -o test.bin
    ```

-   Windows:

    **Note:** The alibabacloud-oss-cpp-sdk.sln project file is not included in the downloaded SDK package. You must run the cmake command to generate the required project file.

    1.  Install CMake. Open cmd to go to the directory that stores the SDK files, create a folder named build, and run cmake to generate the required files, as shown in the following figure.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120374/155851078938252_en-US.png)

        **Note:** To construct project files in the x64 architecture, run the cmake -A x64 .. command.

    2.  Run VS developer command prompt as an administrator. Go to the build folder and run the following command to compile and install the SDK:

        ```
        msbuild ALL_BUILD.vcxproj
        msbuild INSTALL.vcxproj
        ```

        You can also use Visual Studio to open alibabacloud-oss-cpp-sdk.sln to generate a solution.

-   Android:

    Build a project based on the android-ndk-r16 tool chain in Linux. Install third-party libraries libcurl and libopenssl in $ANDROID\_NDK/sysroot and run the following command to compile and install the SDK:

    ```
    cmake -DCMAKE_TOOLCHAIN_FILE=$ANDROID_NDK/build/cmake/android.toolchain.cmake  \
          -DANDROID_NDK=$ANDROID_NDK    \
          -DANDROID_ABI=armeabi-v7a     \
          -DANDROID_TOOLCHAIN=clang     \
          -DANDROID_PLATFORM=android-21 \
          -DANDROID_STL=c++_shared ..
    make
    ```

-   Mac:

    In Mac, you must specify the path where OpenSSL is installed.

    For example, if OpenSSL is installed in /usr/local/Cellar/openssl/1.0.2p, run the following command to compile and install the SDK:

    ```
    cmake -DOPENSSL_ROOT_DIR=/usr/local/Cellar/openssl/1.0.2p  \
          -DOPENSSL_LIBRARIES=/usr/local/Cellar/openssl/1.0.2p/lib  \
          -DOPENSSL_INCLUDE_DIRS=/usr/local/Cellar/openssl/1.0.2p/include/ ..
    make
    ```


