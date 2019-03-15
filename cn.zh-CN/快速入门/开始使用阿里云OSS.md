# 开始使用阿里云OSS {#concept_ptc_g24_tdb .concept}

阿里云对象存储服务（Object Storage Service，简称 OSS）为您提供基于网络的数据存取服务。使用 OSS，您可以通过网络随时存储和调用包括文本、图片、音频和视频等在内的各种非结构化数据文件。

初次使用阿里云 OSS，请您先了解[阿里云 OSS 使用限制](../../../../../cn.zh-CN/产品简介/使用限制.md#)。

阿里云 OSS 将数据文件以对象（object）的形式上传到存储空间（bucket）中。您可以进行以下操作：

-   创建一个或者多个存储空间，向每个存储空间中添加一个或多个文件。
-   通过获取已上传文件的地址进行文件的分享和下载。
-   通过修改存储空间或文件的读写权限（ACL）来设置访问权限。
-   通过阿里云管理控制台、各种便捷工具、以及丰富的 SDK 包执行基本和高级 OSS 操作。

## 使用 OSS 管理控制台 {#section_pw1_4dp_yfb .section}

使用阿里云管理控制台来完成 OSS 基本操作的流程如下：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4330/1552642770919_zh-CN.jpg)

1.  [开通 OSS服务](cn.zh-CN/快速入门/开通OSS服务.md#)
2.  [创建存储空间](cn.zh-CN/快速入门/创建存储空间.md#)
3.  [上传文件](cn.zh-CN/快速入门/上传文件.md#)
4.  [下载文件](cn.zh-CN/快速入门/下载文件.md#)
5.  [删除文件](cn.zh-CN/快速入门/删除文件.md#)
6.  [删除存储空间](cn.zh-CN/快速入门/删除存储空间.md#)

观看以下视频快速了解如何通过 OSS 管理控制台上传下载文件：

## 使用图形化管理工具 ossbrowser {#section_iby_x2p_yfb .section}

ossbrowser 是图形化的 OSS 数据管理工具，支持 Windows、Linux、Mac 平台。使用 ossbrowser，您可以通过图形化界面方便直观地浏览文件、上传下载文件和文件夹（目录）、断点续传、图形化 Policy 授权等操作。因为 ossbrowser 是桌面式图形化工具，所以传输速度和性能不如 ossutil。详情请参见[ossbrowser 快速开始](../../../../../cn.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md#)。

## 使用命令行管理工具 ossutil {#section_lfg_cfp_yfb .section}

ossutil 是以命令行方式管理 OSS 数据的工具，支持 Windows、Linux、Mac 平台。ossutil 提供方便、简洁、丰富的 Bucket 和 Object 管理命令，操作性能好，可并发上传。支持文件和文件夹（目录）上传下载、断点续传等。详情请参见[ossutil快速开始](../../../../../cn.zh-CN/常用工具/命令行工具ossutil/快速开始.md#)。

## 使用 API/SDK {#section_ud2_qxp_yfb .section}

OSS 提供多种语言的 API/SDK 包，方便您快速进行二次开发。详情请参见：

-   [Java SDK 快速入门](../../../../../cn.zh-CN/SDK 参考/Java/快速入门.md#)
-   [Python SDK 快速入门](../../../../../cn.zh-CN/SDK 参考/Python/快速入门.md#)
-   [PHP SDK 快速入门](../../../../../cn.zh-CN/SDK 参考/PHP/快速入门.md#)
-   [Go SDK 快速入门](../../../../../cn.zh-CN/SDK 参考/Go/快速入门.md#)
-   [C SDK 快速入门](../../../../../cn.zh-CN/SDK 参考/C/快速入门.md#)

更多语言的 SDK 示例请参见[OSS SDK 文档](../../../../../cn.zh-CN/SDK 参考/SDK 文档简介.md#)。OSS 各接口的详细信息请参见[OSS API 文档](../../../../../cn.zh-CN/API 参考/简介.md#)。

## 后续操作 {#section_l45_hlq_yfb .section}

OSS 的更多高级操作，请参见[阿里云 OSS 开发指南](../../../../../cn.zh-CN/开发指南/基本概念介绍.md#)。

