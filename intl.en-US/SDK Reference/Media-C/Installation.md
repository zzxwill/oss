# Installation {#concept_32160_zh .concept}

This topic describes how to install Media-C SDK.

## Version dependency { .section}

-   Linux

    OSS C SDK = 3.x.x

-   Windows

    Not supported


## Installation in Linux { .section}

-   Firstly, install the OSS C SDK. For installation steps, see [Install the C SDK](reseller.en-US/SDK Reference/C/Installation.md#).
-   You can [Download SDK package](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32159/cn_zh/1467430514204/OSS_MEDIA_C_SDK_2_0_0.tar.gz) or download [Source Code](https://github.com/aliyun/aliyun-media-c-sdk). The downloaded package includes src, sample, and test directories and the CMakeList.txt file.

-   Install to the system directory

    -   If the OSS C SDK and its dependencies are all installed under the system directory \(/usr/local/ or /usr/\) and you want to install the OSS MEDIA C SDK to the system directory too, you can run the following command to compile and install the SDK:
    ```language-shell
        cmake .
        make
        make install
    
    ```

    -   After the preceding command is successfully run, the OSS MEDIA C SDK is installed to the /usr/local/ directory.
-   Install to the custom directory \(install the dependency package to the system directory\)

    -   If the OSS C SDK and its dependencies are all installed under the system directory \(/usr/local/ or /usr/\), but you want to install the OSS MEDIA C SDK to a custom directory such as /home/user/aliyun/oss/install/, you can run the following command to compile and install the SDK:
    ```language-shell
        cmake . -DCMAKE_INSTALL_PREFIX=/home/user/aliyun/oss/install/usr/local/
        make
        make install
    
    ```

    -   Once you run the preceding command, the OSS MEDIA C SDK is installed to the /home/user/aliyun/oss/install/usr/local/ directory.
-   Install to the custom directory \(install the dependency package to the custom directory\)

    -   If the OSS C SDK or some dependency packages are installed to a custom directory, you cannot find the header files and library files of these packages by default, when you compile the OSS MEDIA C SDK. You must specify the path when you run the cmake command. For example, if you have installed the OSS C SDK to the /home/user/aliyun/oss/install/ directory, run the following command to compile and install the SDK:
    ```language-shell
        cmake . -DCMAKE_INSTALL_PREFIX=/home/user/aliyun/oss/install/usr/local/ -DOSS_C_SDK_INCLUDE_DIR=/home/user/aliyun/oss/install/usr/local/include/ -DOSS_C_SDK_LIBRARY=/home/user/aliyun/oss/install/usr/local/lib/liboss_c_sdk.so
        make
        make install
    
    ```

    -   Once you run the preceding command, the OSS MEDIA C SDK is installed to the /home/user/aliyun/oss/install/usr/local/ directory.
    -   Names of other related parameters for other dependency packages include: APR\_UTIL\_LIBRARY, APR\_LIBRARY, CURL\_LIBRARY, APR\_INCLUDEDIRS, APU\_INCLUDEDIRS, OSS\_C\_SDK\_INCLUDE\_DIR, and CURL\_INCLUDEDIRS.
-   Only compile and install the client SDK

    -   The client SDK and the server SDK are installed at the same time by default. If you only need to compile and install the client SDK, run the following command:
    ```language-shell
        cmake . -DONLY_BUILD_CLIENT=ON
        make
        make install
    
    ```

    -   If you only need to compile and install the server, modify ONLY\_BUILD\_CLIENT to ONLY\_BUILD\_SERVER.
    -   The testing example are compiled only when the client and server SDKs are compiled at the same time.
-   Other compilation and installation methods and issues
    -   Compile mode: Currently four types are supported, namely Debug, Release, MinSizeRef, and RelWithDebInfo. You can specify a compilation type using the parameter -DCMAKE\_BUILD\_TYPE. For example, if you want to use Debug mode for compilation, add the parameter -DCMAKE\_BUILD\_TYPE=Debug: cmake . -DCMAKE\_BUILD\_TYPE=Debug. The Release mode is adopted by default.
        -   Debug: No code optimization is performed. GDB is supported. This mode is generally used for program debugging.
        -   Release: More advanced optimization. This mode is generally applicable to the production environment.
        -   MinSizeRef: This mode generates the smallest library file and is generally used in the embedded environment.
        -   RelWithDebInfo: More advanced optimization approach. This mode carries debugging information and is generally used for the production environment.
    -   If the “Targets may link only to libraries. CMake is dropping the item” warning is prompted during cmake execution, the cause is that the specified library path is incorrect. The library path must be specified to \*.so, such as /path/to/xxx.so.
    -   If you want to use the static library of the OSS C SDK, specify the -DOSS\_C\_SDK\_LIBRARY=/home/user/aliyun/oss/install/usr/local/lib/liboss\_c\_sdk\_static.a during cmake execution. Other libraries are similar.
    -   If “CMake Error: The following variables are used in this project, but they are set to NOTFOUND.” is prompted when you run the cmake, this is because the corresponding library cannot be found in the default path and must be specified. See `Install to a custom directory`.

