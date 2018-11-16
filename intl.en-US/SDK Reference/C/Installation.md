# Installation {#concept_32132_zh .concept}

## Install OSS C SDK in Linux {#section_ggy_4mx_kfb .section}

-   Install CMake and third-party libraries

    When installing OSS C SDK in Linux, you must install CMake and the following third-party libraries: curl, apr, apr-util and minixml.

    The following parameters are required to install the environment:

    |Parameter|Description|Version requirement|
    |:--------|:----------|:------------------|
    |CMake|Build and installation tool|Version 2.6.0 and later|
    |curl|Focuses on network problems.|Version 7.32.0 and later|
    |apr-util|Focuses on memory management and cross-platform operations.|Version 1.5.2 and later|
    |minixml|Parses XML returned by a request.|We recommend you use version 2.9.|

    Select the operation system that you need to install CMake and third-party libraries.

    -   Ubuntu/Debian
        -   Install CMake

            ```language-shell
            sudo apt-get install cmake
            
            ```

        -   Install third-party libraries

            ```language-shell
            sudo apt-get install libcurl4-openssl-dev libapr1-dev libaprutil1-dev libmxml-dev
            
            ```

            If the mxml installation package is not included in the yum source, you can install the libraries using RPM. Download the rpm installation package based on your operation system:

            -   64-bit: [mxml-2.9-1.x86\_64.rpm](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32132/cn_zh/1501596081318/mxml-2.9-1.x86_64.rpm?spm=a2c4g.11186623.2.9.403527b9NiIRgn&file=mxml-2.9-1.x86_64.rpm) 
            -   32-bit: [mxml-2.9-1.i386.rpm](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32132/cn_zh/1501596109140/mxml-2.9-1.i386.rpm) 
            Run the following code to install mxml with the rpm installation package:

            ```language-shell
            rpm -ivh mxml-2.9-1.x86_64.rpm
            
            ```

    -   RedHat/Aliyun/CentOS
        -   Install CMake

            ```language-shell
            sudo yum install cmake
            
            ```

        -   Install third-party libraries

            ```language-shell
            sudo yum install curl-devel apr-devel apr-util-devel  mxml mxml-devel
            
            ```

    -   SuSE
        -   Install CMake

            ```language-shell
            zypper install cmake
            
            ```

        -   Install third-party libraries

            ```language-shell
            zypper install libcurl-devel libapr1-devel libapr-util1-devel mxml-devel
            
            ```

    -   Other Linux
        -   Install CMake: [Download link](https://cmake.org/download)

            The common installation method is as follows:

            ```language-shell
            ./configure
            make
            make install
            
            ```

            **Note:** When running ./configure, the default installation path is /usr/local/. If you want to specify another path, use the ./configure --prefix option.

        -   Install libcurl: [Download link](http://curl.haxx.se/download.html)

            The common installation method is as follows:

            ```language-shell
            ./configure
            make
            make install
            
            ```

        -   Install apr: [Download link](https://apr.apache.org/download.cgi)

            The common installation method is as follows:

            ```language-shell
            ./configure
            make
            make install
            
            ```

        -   Install apr-util: [Download link](https://apr.apache.org/download.cgi)

            The common installation method is as follows:

            ```language-shell
            // You must specify the --with-apr option in the installation.
            ./configure --with-apr=/your/apr/install/path
            make
            make install
            
            ```

        -   Install minixml: [Download link](http://michaelrsweet.github.io/mxml/)

            The common installation method is as follows:

            ```language-shell
            ./configure
            make
            sudo make install
            
            ```

-   Download SDK
    -    [Download SDK directly](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32131/cn_zh/1501595738954/aliyun-oss-c-sdk-3.5.0.tar.gz?spm=a2c4g.11186623.2.16.7f6327b9B1Jhba&file=aliyun-oss-c-sdk-3.5.0.tar.gz)
    -    [Download SDK from GitHub](https://github.com/aliyun/aliyun-oss-c-sdk?spm=a2c4g.11186623.2.17.7f6327b9B1Jhba) 
    -    [Download historical SDK](https://github.com/aliyun/aliyun-oss-c-sdk/releases) 
-   Install SDK

    Use cmake. If third-party libraries, such as url, apr, apr-util and mxml are not installed in the default method during the compiling process, you must specify the path of their header files and library files.

    Install the SDK as follows:

    ```language-shell
    cmake .
    make
    make install
    
    ```

    An installation example in CentOS is as follows:

    ```language-shell
    cmake -f CMakeLists.txt
    // The compilation type is Release. Common compilation types include: Debug, Release, RelWithDebInfo, and MinSizeRel, in which Debug is the default type.
    -DCMAKE_BUILD_TYPE=Release
    // Customize the installation directory.
    -DCMAKE_INSTALL_PREFIX=/usr/local/
    // Specify the path where the header files and library files of third party libaries, such as curl, apr, apr-util, and xml, are located,
    -DCURL_INCLUDE_DIR=/usr/include/curl
    -DCURL_LIBRARY=/usr/lib64/libcurl.so
    -DAPR_INCLUDE_DIR=/usr/include/apr-1
    -DAPR_LIBRARY=/usr/lib64/libapr-1.so
    -DAPR_UTIL_INCLUDE_DIR=/usr/include/apr-1
    -DAPR_UTIL_LIBRARY=/usr/lib64/libaprutil-1.so
    -DMINIXML_INCLUDE_DIR=/usr/include
    -DMINIXML_LIBRARY=/usr/lib64/libmxml.so
    // If the "Could not find apr-config/apr-1-config" error occurs during the compiling process, it indicates that the apr-1-config file cannot be found in the default path. Add this option.
    -DAPR_CONFIG_BIN=/path/to/bin/apr-1-config
    // If the "Could not find apu-config/apu-1-config" error occurs, add this option.
    -DAPU_CONFIG_BIN=/path/to/bin/apu-1-config
    
    ```


## Install OSS C SDK in Windows { .section}

-   Download SDK
    -    [Download SDK directly](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32131/cn_zh/1501595775239/aliyun-oss-c-sdk-3.5.0.zip) 
    -    [Download SDK from GitHub](https://github.com/aliyun/aliyun-oss-c-sdk) 
    -    [Download historical SDK](https://github.com/aliyun/aliyun-oss-c-sdk/releases) 
-   Install SDK

    For detailed steps and common problems when using Visual Studio to compile OSS C SDK, see [Compile and Use Alibaba Cloud OSS C SDK in Windows](https://yq.aliyun.com/articles/57947).

    **Note:** If you use Visual Studio 2012 and later versions, it prompts you to upgrade the project to the latest compiler and libraries. We recommended you to determine whether to upgrade based on your project. You can upgrade if your project uses the latest compiler and libraries.


