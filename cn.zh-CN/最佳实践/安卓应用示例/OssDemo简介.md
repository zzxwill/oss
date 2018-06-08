# OssDemo简介 {#concept_ivt_2mf_vdb .concept}

在 OSS 的开发人员指南中介绍了[移动端开发上传场景](../cn.zh-CN/开发指南/接入OSS/基于OSS的移动开发.md#)。本文主要是基于这个场景来介绍在Android上如何使用SDK来实现一些常见的操作，也就是OssDemo这个工程。主要是以下几个方面：

-   如何使用已经搭建好的应用服务器（STS）
-   如何使用SDK来上传文件
-   如何使用图片服务

这里假设您对OSS的移动开发场景有所了解，并且知道STS（Security Token Service）。

## 准备工作 {#section_ln3_gmf_vdb .section}

由于是基于Android的开发，所以需要做一些准备工作。

1.  开通OSS。请参考 [快速入门](../cn.zh-CN/快速入门/开始使用阿里云OSS.md#)。
2.  搭建应用服务器。请参考 [快速搭建移动应用直传服务](cn.zh-CN/最佳实践/移动应用端直传实践/快速搭建移动应用直传服务.md#)。
3.  准备Android开发环境。这里主要用的是Android Studio，网上有很多教程，这里就不再重复。
4.  下载OssDemo的源码。[OssDemo源码下载地址](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sdk/OssDemo_2016-01-19.zip)。可以安装后体验，后面源码分析有详细介绍上面提到的常见操作的实现。
5.  打开OSS官方提供的 [OSS Android SDK文档](https://help.aliyun.com/document_detail/32043.html) 以供参考。

