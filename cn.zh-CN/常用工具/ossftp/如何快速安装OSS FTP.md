# 如何快速安装OSS FTP {#concept_d4b_g2t_vdb .concept}

OSS FTP工具是一个特殊的FTP server。它接收普通FTP请求后，将对文件、文件夹的操作映射为对OSS的操作，从而使得您可以基于FTP协议来管理存储在OSS上的文件。

**说明：** OSS FTP工具主要面向个人用户使用，生产环境请使用OSS SDK。

## 功能简介 {#section_ntf_h2t_vdb .section}

-   主要特性
    -   跨平台：无论是Windows、Linux还是Mac，无论是32位还是64位操作系统，无论是图形界面还是命令行都可以运行。
    -   免安装： 解压后可直接运行。
    -   免设置：无需设置即可运行。
    -   透明化： FTP工具是Python写的，您可以看到完整的源码，我们稍后也会开源到Github。
-   主要功能
    -   支持文件和文件夹的上传、下载、删除等操作。
    -   通过Multipart方式，分片上传大文件。
    -   支持大部分FTP指令，可以满足日常FTP的使用需求。

        **说明：** 

        -   在目前的1.0版本中，考虑到安装部署的简便，OSS FTP工具不支持TLS加密。由于FTP协议是明文传输的，为了防止您的密码泄漏，建议将FTP Server和Client运行在同一台机器上，通过127.0.0.1:port的方式来访问。
        -   不支持rename和move操作。
        -   安装包解压后的路径不能包含中文。
        -   FTP Server的管理控制页面在低版本的IE中可能打不开。
        -   FTP Server支持的Python版本：Python2.6和Python2.7。

## 下载地址 {#section_xj4_vpt_vdb .section}

-   Windows：[ossftp-1.0.3-win.zip](http://gosspublic.alicdn.com/ossftp/ossftp-1.0.3-win.zip) 

    Windows默认不会安装Python2.7，所以安装包中包含了Python2.7，解压后即可使用。

-   Linux/Mac：[ossftp-1.0.3-linux-mac.zip](http://gosspublic.alicdn.com/ossftp/ossftp-1.0.3-linux-mac.zip) 

    Linux/Mac系统默认安装Python2.7或Python2.6，所以安装包中不再包含可执行的Python,，只包含了相关依赖库。


## 快速运行 {#section_brg_bqt_vdb .section}

1.  下载和您系统匹配的安装包。
2.  将安装包解压后运行。
    -   Windows系统

        解压后双击运行start.vbs即可。如双击无反应，请升级您的IE浏览器或设置其他浏览器为默认浏览器。

    -   Linux系统
        1.  解压下载的文件：

            ``` {#codeblock_zed_8nh_cly}
            $ unzip ossftp-1.0.3-linux-mac.zip
            ```

        2.  进入解压后的文件夹，运行start.sh：

            ``` {#codeblock_hp1_ush_gi2}
            $ cd ossftp-1.0.3-linux-mac
            $ bash start.sh
            ```

        3.  使用一台可以访问这台服务器的，有图形化界面的电脑，通过浏览器访问FTP服务器操作界面。访问域名：`http://ServerIP:8192`。
    -   Mac系统

        解压后双击start.command，或者在终端运行`$ bash start.command`。

3.  上述步骤会启动一个FTP server，默认监听在127.0.0.1的2048端口。同时，为了方便您对FTP server的状态进行管控，还会启动一个web服务器，监听在127.0.0.1的8192端口。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4864/15592952692519_zh-CN.jpg)

    -   **ossftp监听地址**：填写需要使用FTP服务的客户端IP。如果是在本机上运行客户端，保持默认即可。
    -   **ossftp监听端口**：ossftp的监听端口，默认即可。
    -   **ossftp日志等级**：ossftp的日志输出等级，根据需求填写。
    -   **Bucket endpoints**：填写Bucket的访问域名（格式为bucket\_name.enpoint），多个域名以英文逗号（,）隔开。
    -   **Language**：选择ossftp的显示语言。
    **说明：** 

    -   配置完成后需保存配置并重启生效。
    -   同一时间内只能存在一个服务器和一个连接。如果在一个服务器已连接的情况下新建连接，则之前连接会直接断开。
4.  下载[FileZilla客户端](https://filezilla-project.org/?spm=a2c4g.11186623.2.6.bqHidZ)并安装。
5.  配置OSS访问信息后单击**快速连接**：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4864/15592952692520_zh-CN.png)

    -   **主机**：配置服务器IP地址，若服务器和客户端在一台设备上，使用默认地址：127.0.0.1。
    -   **用户名**：填写拥有Bucket访问权限的AccessKeyID和Bucket名称，格式为AccessKeyID/bucket\_name，例如`tSxyi******wPMEp/test-hz-jh-002`。
    -   **密码**：填写拥有Bucket访问权限的AccessKeySecret。
    -   **端口**：填写服务器配置的监听端口，默认填写2048。

        **说明：** AccessKeyID和AccessKeySecret的获取，请参见[创建RAM用户](../../../../cn.zh-CN/快速入门/创建 RAM 用户.md#section_zt4_dcf_xdb)中的创建AccessKey部分。


## 高级应用 {#section_xdq_krt_vdb .section}

-   通过控制页面管理FTP Server

    -   修改监听地址

        如果通过网络来访问FTP Server，需要修改监听地址。因为默认的监听地址127.0.0.1只允许来自本地的访问。可以修改成内网IP或公网IP。

    -   修改监听端口

        修改FTP Server监听的端口，建议监听端口大于1024。 因为监听1024以下的端口时需要管理员权限。

    -   修改日志等级

        设置FTP Server的日志级别。FTP Server的日志会输出到`data/ossftp/`目录下， 可以通过控制页面的日志按钮在线查看。默认的日志级别为INFO，打印的日志信息较少。如果需要更详细的日志信息，可以修改为DEBUG模式。如果希望减少日志的输出，可以设置级别为WARNING或ERROR等。

    -   设置Bucket endpoints

        FTP server默认会探索Bucket的所属location信息，随后将请求发到对应的Region（如`oss-cn-hangzhou.aliyuncs.com`或`oss-cn-beijing.aliyuncs.com`\)，FTP Server会优先尝试内网访问OSS。如果您设置了Bucket endpoints，如设置为`test-bucket-a.oss-cn-hangzhou.aliyuncs.com`， 那么当访问test-bucket-a时，就会使用`oss-cn-hangzhou.aliyuncs.com`域名。

    -   设置显示语言

        通过设置cn/en，可修改FTP控制页面的显示语言为中文/英文。

    **说明：** 

    -   所有修改都需要重启才能生效。
    -   上述的所有修改本质上都是修改ftp根目录下的config.json， 所以您可以直接修改该文件。
-   直接启动FTP Server\(Linux/Mac\)

    可以直接启动ossftp目录下的ftpserver.py，免去web\_server的开销。

    ``` {#codeblock_cf1_mac_d7v}
    $ python ossftp/ftpserver.py &
    ```

    配置修改方式同上。


