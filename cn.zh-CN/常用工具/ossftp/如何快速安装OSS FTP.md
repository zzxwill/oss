# 如何快速安装OSS FTP {#concept_d4b_g2t_vdb .concept}

## 简介 {#section_ntf_h2t_vdb .section}

OSS FTP工具是一个特殊的FTP server。它接收普通FTP请求后，将对文件、文件夹的操作映射为对OSS的操作，从而使得您可以基于FTP协议来管理存储在OSS上的文件。

**说明：** 生产环境请使用OSS SDK，OSS FTP工具主要面向个人用户使用。

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

        1.  目前在1.0版本中，考虑到安装部署的简便，OSS FTP工具不支持TLS加密。由于FTP协议是明文传输的，为了防止您的密码泄漏，建议将FTP server和client运行在同一台机器上，通过127.0.0.1:port的方式来访问。
        2.  不支持rename和move操作。
        3.  安装包解压后的路径不能包含中文。
        4.  FTP server的管理控制页面在低版本的IE中可能打不开。
        5.  FTP server支持的Python版本：Python2.6和Python2.7。

## 下载 {#section_xj4_vpt_vdb .section}

-   Windows：[ossftp-1.0.3-win.zip](http://gosspublic.alicdn.com/ossftp/ossftp-1.0.3-win.zip)

    Windows默认不会安装Python2.7，所以安装包中包含了Python2.7，解压后即可使用。

-   Linux/Mac：[ossftp-1.0.3-linux-mac.zip](http://gosspublic.alicdn.com/ossftp/ossftp-1.0.3-linux-mac.zip)

    Linux/Mac系统默认安装Python2.7或Python2.6，所以安装包中不再包含可执行的Python,，只包含了相关依赖库。


## 运行 {#section_brg_bqt_vdb .section}

首先解压之前下载的文件，然后根据环境情况选择不同的运行方式。

-   Windows：双击运行start.vbs即可
-   Linux： 打开终端，运行`$ bash start.sh`
-   Mac：双击start.command，或者在终端运行`$ bash start.command`

上述步骤会启动一个FTP server，默认监听在127.0.0.1的2048端口。同时，为了方便您对FTP server的状态进行管控，还会启动一个web服务器，监听在127.0.0.1的8192端口。如果您的系统有图形界面，还会自动打开控制页面。在控制页面中允许修改监听地址、监听端口、日志类型、指定某个地域的存储空间（格式为bucket.enpoint）、页面语言。修改后需保存配置并在重启后生效。

**说明：** 同一时间内只能存在一个服务器和一个连接。如果在一个服务器已连接的情况下新建连接，则之前连接会直接断开。

## 连接到FTP server {#section_uwr_rqt_vdb .section}

请使用[FileZilla客户端](https://filezilla-project.org/?spm=a2c4g.11186623.2.6.bqHidZ)连接FTP server。下载安装后，按如下方式连接即可：

-   主机：127.0.0.1
-   登录类型： 账号
-   用户：格式为access\_key\_id/bucket\_name，例如tSxyiUM3NKswPMEp/test-hz-jh-002
-   密码：access\_key\_secret
-   账号：使用默认值

    **说明：** access\_key\_id和access\_key\_secret的获取，请参见[创建RAM用户](../../../../intl.zh-CN/快速入门/创建 RAM 用户.md#section_zt4_dcf_xdb)中的创建AK部分。


## 高级使用 {#section_xdq_krt_vdb .section}

-   通过控制页面管理FTP server

    -   修改监听地址

        如果通过网络来访问FTP server，需要修改监听地址。因为默认的监听地址127.0.0.1只允许来自本地的访问。可以修改成内网IP或公网IP。

    -   修改监听端口

        修改FTP server监听的端口，建议监听端口大于1024。 因为监听1024以下的端口时需要管理员权限。

    -   修改日志等级

        设置FTP server的日志级别。FTP server的日志会输出到`data/ossftp/`目录下， 可以通过控制页面的日志按钮在线查看。默认的日志级别为INFO，打印的日志信息较少。如果需要更详细的日志信息，可以修改为DEBUG模式。如果希望减少日志的输出，可以设置级别为WARNING或ERROR等。

    -   设置Bucket endpoints

        FTP server默认会探索Bucket的所属location信息，随后将请求发到对应的region（如`oss-cn-hangzhou.aliyuncs.com`或`oss-cn-beijing.aliyuncs.com`\)，FTP server会优先尝试内网访问oss。如果您设置了Bucket endpoints，如设置为`test-bucket-a.oss-cn-hangzhou.aliyuncs.com`， 那么当访问test-bucket-a时，就会使用`oss-cn-hangzhou.aliyuncs.com`域名。

    -   设置显示语言

        通过设置cn/en，可修改FTP控制页面的显示语言为中文/英文。

    **说明：** 

    -   所有修改都需要重启才能生效。
    -   上述的所有修改本质上都是修改ftp根目录下的config.json， 所以您可以直接修改该文件。
-   直接启动FTP server\(Linux/Mac\)

    可以直接启动ossftp目录下的ftpserver.py，免去web\_server的开销。

    ```
    $ python ossftp/ftpserver.py &
    ```

    配置修改方式同上。


## 可能遇到的问题 {#section_g3r_zrt_vdb .section}

-   如果连接FTP server时，遇到以下错误：

    有两种可能：

    -   输入的 access\_key\_id 和 access\_key\_secret有误。

        解决：请输入正确的信息后再重试。

    -   所用的access\_key信息为ram 子账户的access\_key，而子账户不具有List buckets权限。

        解决：当使用子账户访问时，请在控制页面中指定bucket endpoints， 即告诉FTP server某个bucket应该用哪个endpoint来访问。同时，子账户也需要一些必须的权限。关于使用ram访问oss时的访问控制，请参考文档[访问控制](../../../../intl.zh-CN/开发指南/访问与控制/访问控制.md#)。具体如下。

        -   只读访问

            OSS FTP工具需要的权限为 ListObjects、GetObject、HeadObject。关于如何创建一个具有只读访问的ram子账户，请参考图文教程[如何结合ram实现文件共享](intl.zh-CN/常用工具/ossftp/如何结合RAM实现文件共享.md#)。

        -   上传文件

            如果允许ram子账户上传文件，还需要PutObject权限。

        -   删除文件

            如果允许ram子账户删除文件，还需要DeleteObject权限。

-   如果您在Linux下运行FTP server，然后用FileZilla连接时遇到如下错误：

    ```
    501 can't decode path (server filesystem encoding is ANSI_X3.4-1968)
    ```

    此类错误通常是因为本地的中文编码有问题。在将要运行start.sh的终端中输入下面的命令，然后再重新启动即可。

    ```
    $ export LC_ALL=en_US.UTF-8; export LANG="en_US.UTF-8"; locale
    ```


