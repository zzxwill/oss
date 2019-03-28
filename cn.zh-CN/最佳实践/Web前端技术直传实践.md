# Web前端技术直传实践 {#concept_xjz_zyp_fhb .concept}

本教程介绍如何利用 Web 前端技术将文件上传到 OSS。

## 背景信息 {#section_wqp_pzp_fhb .section}

每个 OSS 的用户都会用到上传服务。Web 端常见的上传方法是用户在浏览器或 APP 端上传文件到应用服务器，应用服务器再把文件上传到 OSS。具体流程如下图所示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150141/155375369741859_zh-CN.png)

和数据直传到 OSS 相比，以上方法有三个缺点：

-   上传慢：用户数据需先上传到应用服务器，之后再上传到OSS。网络传输时间比直传到OSS多一倍。如果用户数据不通过应用服务器中转，而是直传到OSS，速度将大大提升。而且OSS采用BGP带宽，能保证各地各运营商之间的传输速度。
-   扩展性差：如果后续用户多了，应用服务器会成为瓶颈。
-   费用高：需要准备多台应用服务器。由于OSS上传流量是免费的，如果数据直传到OSS，不通过应用服务器，那么将能省下几台应用服务器。

## 技术方案 {#section_stb_rzp_fhb .section}

目前通过 Web 前端技术上传文件到 OSS，有三种技术方案：

-   利用OSS Browser.js SDK 将文件上传到 OSS

    该方案通过OSS Browser.js SDK直传数据到 OSS，详细的 SDK Demo 请参考[上传文件](../../../../../cn.zh-CN/SDK 参考/Browser.js/上传文件.md#)。在网络条件不好的状况下可以通过断点续传的方式上传大文件。该方案在个别浏览器上有兼容性问题，目前兼容 IE10 及以上版本浏览器，主流版本的 Edge、Chrome、Firefox、Safari 浏览器，以及大部分的 Android、iOS、WindowsPhone 手机上的浏览器。更多信息请参见[安装 Browser.js SDK](../../../../../cn.zh-CN/SDK 参考/Browser.js/安装.md#)。

    **说明：** Browser.js SDK 已经支持大部分的 OSS API 接口，包含文件管理、自定义域名设置、图片处理等。

-   使用表单上传方式，将文件上传到 OSS

    利用 OSS 提供的 PostObject 接口，使用表单上传方式将文件上传到 OSS。该方案兼容大部分浏览器，但在网络状况不好的时候，如果单个文件上传失败，只能重试上传。操作方法请参见 [PostObject 上传方案](cn.zh-CN/最佳实践/Web端直传实践/Web端直传实践简介.md#)。

    **说明：** 关于 PostObject 的详细介绍请参见 [PostObject](../../../../../cn.zh-CN/API 参考/关于Object操作/PostObject.md#)。

-   通过小程序上传文件到 OSS

    通过小程序，如微信小程序、支付宝小程序等，利用 OSS 提供的 PostObject 接口来实现表单上传。操作方式请参见[小程序上传实践](cn.zh-CN/最佳实践/小程序直传实践.md#)。


