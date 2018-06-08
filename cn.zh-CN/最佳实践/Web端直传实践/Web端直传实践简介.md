# Web端直传实践简介 {#concept_iyn_vfy_5db .concept}

## 目的 {#section_cym_wfy_5db .section}

本教程的目的是通过两个例子介绍如何在Html表单提交直传OSS。

-   第一个例子：讲解签名在服务端（php）完成，然后直接通过表单上传到OSS。
-   第二个例子：讲解签名在服务端（php）完成，并且服务端设置了上传后回调。然后直接通过表单上传到OSS，OSS回调完应用服务器再返回给用户。

## 背景 {#section_m3m_xfy_5db .section}

每个OSS的用户，都会用到上传。由于是网页上传，其中包括一些APP里面的h5页面，对上传的需求很强烈，很多人采用的做法是用户在浏览器/APP上传到应用服务器，然后应用服务器再把文件上传到OSS。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4403/1459_zh-CN.png)

这种方法有三个缺点：

-   第一：上传慢。先上传到应用服务器，再上传到OSS，网络传送多了一倍。如果数据直传到OSS，不走应用服务器，速度将大大提升，而且OSS是采用BGP带宽，能保证各地各运营商的速度。
-   第二：扩展性不好。如果后续用户多了，应用服务器会成为瓶颈。
-   第三：费用高。由于OSS上传流量是免费的。如果数据直传到OSS，不走应用服务器，那么将能省下几台应用服务器。

## 基础篇：应用服务器php返回签名 {#section_rpn_hgy_5db .section}

[单击这里打开示例](cn.zh-CN/最佳实践/Web端直传实践/服务端签名后直传.md#)

## 进阶篇：应用服务器php返回签名及采用上传回调 {#section_p4n_3gy_5db .section}

[单击这里打开示例](cn.zh-CN/最佳实践/Web端直传实践/服务端签名直传并设置上传回调.md#)

