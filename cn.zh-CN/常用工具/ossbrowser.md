# ossbrowser {#concept_xmg_h33_wdb .concept}

ossbrowser是OSS官方提供的图形化的管理工具，提供类似Windows资源管理器的功能。您可以方便地浏览文件、上传/下载文件，并支持断点续传。

ossbrowser主要功能包括：支持AK登录、临时授权码登录，管理Bucket，管理文件，提供Policy授权，生成STS临时授权。

## 下载安装 {#section_ppm_j33_wdb .section}

|支持平台|下载地址|
|:---|:---|
|Window x32|[Window x32](https://github.com/aliyun/oss-browser/blob/master/all-releases.md)|
|Window x64|[Window x64](https://github.com/aliyun/oss-browser/blob/master/all-releases.md)|
|MAC|[MAC](https://github.com/aliyun/oss-browser/blob/master/all-releases.md)|
|Linux x64|[Linux x64](https://github.com/aliyun/oss-browser/blob/master/all-releases.md)|

## 登录ossbrowser {#section_mq4_l33_wdb .section}

-   使用AK登录ossbrowser

    您可以使用AK（比如子账号AK）登录使用ossbrowser。

    **说明：** 不推荐使用主账号AK登录。

    1.  登录[RAM控制台](https://ram.console.aliyun.com/)创建子账号。

        子帐号的权限分为：

        -   大权限子账号（即拥有所有Bucket权限，且可以管理RAM配置的子账号）。初级用户推荐如下配置：

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/6122_zh-CN.png)

            **说明：** 您可以为子帐号授予更小的权限，具体设置请参考[权限管理](../cn.zh-CN//权限管理概述.md#)。

        -   小权限子账号（即只拥有部分Bucket或子目录的权限）。初级用户推荐使用[简化Policy授权](#section_zyx_1k3_wdb)功能完成授权。
    2.  使用子帐号登录ossbrowser。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/2995_zh-CN.png)

        **说明：** 

        -   预设OSS路径（可选）：当前使用的AK只有某个Bucket或Bucket下某个路径的权限，需要设置预设OSS路径和区域。
        -   记住秘钥：勾选记住秘钥可保存AK秘钥。再次登录时，单击**AK历史**，可选择该密钥登录，不需要手动输入AK。请不要在临时使用的电脑上勾选。
-   使用临时授权码登录ossbrowser

    OSSBrowser还支持临时授权码登录。适用场景：您可以将临时授权码提供给相应的人员，允许其在授权码到期前，临时访问您Bucket下某个目录。到期后，临时授权码会自动失效。

    -   生成临时授权码

        您作为管理员，先使用AK登录OSSBrowser，并在管理界面，选择需要临时授权的文件或目录，生成临时授权码。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/3006_zh-CN.png)

    -   登录时使用授权码

        临时授权码可在过期前，可用来登录OSSBrowser。如下图所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/3007_zh-CN.png)


## 管理Bucket {#section_idm_xj3_wdb .section}

登录ossbrowser后，您可以管理Bucket，包括：

-   新建Bucket
-   删除Bucket
-   修改Bucket权限
-   管理碎片

## 管理文件 {#section_nm5_yj3_wdb .section}

ossbrowser提供的文件管理功能包括：

-   目录（包括Bucket）和文件的增加、删除、修改、搜索、复制、文件预览

-   文件传输任务管理：上传（支持拖拽操作）、下载、断点续传

-   地址栏功能：支持`oss://`协议URL、浏览历史前进后退、保存书签

-   归档型存储的管理：创建归档型存储、恢复归档型存储Bucket

    **说明：** 归档存储型Bucket下所有文件均为Archive存储类型，需要恢复才能访问。


## 简化Policy授权 {#section_zyx_1k3_wdb .section}

1.  勾选一个或多个需要授权的文件或目录，并单击**简化Policy授权**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/2998_zh-CN.png)

2.  在简化Policy授权窗口，选择权限。
3.  您可以查看生成的Policy文本，将其复制到您需要的地方使用，如RAM子账号、Role等Policy编辑等。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/2999_zh-CN.png)

    您也在该窗口中将权限授权给子账号（当前登录的AK必须有RAM的配置操作权限）。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/3000_zh-CN.png)


## 生成STS临时授权 {#section_e2m_nk3_wdb .section}

1.  勾选文件夹，并选择生成授权码。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/3004_zh-CN.png)

2.  在弹出的窗口中，设置参数并单击**重新生成**，将会生成授权码，如下图所示。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/3005_zh-CN.png)


## 相关文档 {#section_m3r_4l3_wdb .section}

-   [创建AccessKey](https://help.aliyun.com/document_detail/53045.html)
-   [权限管理](../cn.zh-CN//权限管理概述.md#)
-   [STS临时授权](../cn.zh-CN//STS临时授权访问.md#)
-   [控制台-管理bucket](../cn.zh-CN//创建存储空间.md#)
-   [Java SDK-管理bucket](https://help.aliyun.com/document_detail/32012.html)
-   [API-创建bucket](../cn.zh-CN/API 参考/关于Bucket的操作/PutBucket.md#)

