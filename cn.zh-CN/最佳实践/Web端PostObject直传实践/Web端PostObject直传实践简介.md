# Web端PostObject直传实践简介 {#concept_iyn_vfy_5db .concept}

本教程介绍如何在Web端通过表单上传方式直接上传数据到OSS。

每个OSS用户都会用到上传服务。Web端常见的上传方法是用户在浏览器或APP端上传文件到应用服务器，应用服务器再把文件上传到OSS。这种方式需通过应用服务器中转，传输效率明显低于数据直转至OSS的方式。

数据直转至OSS的方式是利用OSS提供的[PostObject](../../../../../cn.zh-CN/API 参考/关于Object操作/PostObject.md#)接口，使用表单上传方式将文件上传到 OSS。您可以通过以下案例了解如何通过表单上传的方式，直传数据到OSS：

-   在客户端通过JavaScript代码完成签名，然后通过表单直传数据到OSS。详情请参见[JavaScript客户端签名直传](cn.zh-CN/最佳实践/Web端PostObject直传实践/JavaScript客户端签名直传.md#)。
-   在服务端完成签名，然后通过表单直传数据到OSS。详情请参见[服务端签名后直传](cn.zh-CN/最佳实践/Web端PostObject直传实践/服务端签名后直传.md#)。
-   在服务端完成签名，并且服务端设置了上传后回调，然后通过表单直传数据到OSS。OSS回调完成后，再将应用服务器响应结果返回给客户端。详情请参见[服务端签名直传并设置上传回调](cn.zh-CN/最佳实践/Web端PostObject直传实践/服务端签名直传并设置上传回调/原理介绍.md#)。

