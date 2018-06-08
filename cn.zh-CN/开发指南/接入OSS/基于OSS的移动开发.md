# 基于OSS的移动开发 {#concept_e4j_x51_5db .concept}

## 开发架构图 {#section_azy_z51_5db .section}

典型的基于OSS的移动开发有四个组件：

-   OSS：提供上传、下载、上传回调等功能。
-   开发者的移动客户端（App或者网页应用），简称客户端：通过开发者提供的服务，间接使用OSS。
-   应用服务器：客户端交互的服务器。也是开发者的业务服务器。
-   阿里云STS：颁发临时凭证。

## 最佳实践 {#section_ahj_bv1_5db .section}

-   [30分钟快速搭建移动应用直传服务](../cn.zh-CN/最佳实践/移动应用端直传实践/快速搭建移动应用直传服务.md#)
-   [移动端回调案例分析及代码下载](../cn.zh-CN/最佳实践/移动应用端直传实践/快速搭建移动应用上传回调服务.md#)
-   [轻松搞定移动端安全策略](../cn.zh-CN/最佳实践/移动应用端直传实践/权限控制.md#)

## 开发业务流程 {#section_ims_dv1_5db .section}

-   临时凭证授权的上传

    如图所示：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4352/1027_zh-CN.png)

    具体步骤如下：

    1.  客户端向应用服务器发出上传文件到OSS的请求。
    2.  应用服务器向STS服务器请求一次，获取临时凭证。
    3.  应用服务器回复客户端，将临时凭证返回给客户端。
    4.  客户端获取上传到OSS的授权（STS的AccessKey以及Token），调用OSS提供的移动端SDK上传。
    5.  客户端成功上传数据到OSS。如果没有设置回调，则流程结束。如果设置了回调功能，OSS会调用相关的接口。
    这里有几个要点：

    -   客户端不需要每次都向应用服务器请求授权，在第一次授权完成之后可以缓存STS返回的临时凭证直到超过失效时间。
    -   STS提供了强大的权限控制功能，可以将客户端的访问权限限制到Object级别，从而实现不同客户端在OSS端上传的Object的完全隔离，极大的提高了安全系数。
    更多信息请参见 [授权给第三方上传](cn.zh-CN/开发指南/上传文件/授权给第三方上传.md#)。

-   签名URL授权的上传和表单上传

    如图所示：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4352/1030_zh-CN.png)

    具体步骤如下：

    1.  客户端向应用服务器发出上传文件到OSS的请求。
    2.  应用服务器回复客户端，将上传凭证（签名URL或者表单）返回给客户端。
    3.  客户端获取上传到OSS的授权（签名URL或者表单），调用OSS提供的移动端SDK上传或者直接表单上传。
    4.  客户端成功上传数据到OSS。如果没有设置回调，则流程结束。如果设置了回调功能，OSS会调用相关的接口。
    更多信息请参见 [授权给第三方上传](cn.zh-CN/开发指南/上传文件/授权给第三方上传.md#)。

-   临时凭证授权的下载

    跟临时凭证授权的上传过程类似：

    1.  客户端向应用服务器发出下载OSS文件的请求。
    2.  应用服务器向STS服务器请求一次，获取临时凭证。
    3.  应用服务器回复客户端，将临时凭证返回给客户端。
    4.  客户端获取下载OSS文件的授权（STS的AccessKey以及Token），调用OSS提供的移动端SDK下载。
    5.  客户端成功从OSS下载文件。
    这里有几个要点：

    -   和上传类似，客户端对于临时凭证也可以进行缓存从而提高访问速度。
    -   STS同样也提供了精确到Object的下载权限控制，和上传权限控制结合在一起可以实现各移动端在OSS上存储空间的完全隔离。
-   签名URL授权的下载

    跟签名URL授权的上传类似：

    1.  客户端向应用服务器发出下载OSS文件的请求。
    2.  应用服务器回复客户端，将签名URL返回给客户端。
    3.  客户端获取下载OSS文件的授权（签名URL），调用OSS提供的移动端SDK下载。
    4.  客户端成功从OSS下载文件。
    **说明：** 客户端不能存储开发者的AccessKey，只能获取应用服务器签名的URL或者是通过STS颁发的临时凭证（也就是STS的AccessKey和Token）。


## 最佳实践 {#section_sjx_ww1_5db .section}

-   [RAM和STS使用指南](../cn.zh-CN/最佳实践/权限管理/权限管理概述.md#)

## 功能使用参考 {#section_ip1_zw1_5db .section}

-   SDK：Android SDK [上传文件](https://help.aliyun.com/document_detail/32047.html)
-   SDK：Android SDK [上传文件](https://www.alibabacloud.com/help/doc-detail/32047.htm)
-   SDK：iOS SDK [上传文件](https://help.aliyun.com/document_detail/32060.html)

