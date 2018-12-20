# OSS请求流程 {#concept_xcz_ff5_tdb .concept}

对OSS的HTTP请求可以根据是否携带身份验证信息分为匿名请求和带身份验证的请求。匿名请求指的是请求中没有携带任何和身份相关的信息；带身份验证的请求指的是按照 OSS API 文档中规定的在请求头部或者在请求 URL 中携带签名的相关信息。

## 匿名请求流程 {#section_dk1_gx2_2gb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4345/1545295120959_zh-CN.png)

1.  用户的请求被发送到OSS的HTTP服务器上。
2.  OSS根据URL解析出Bucket和Object。
3.  OSS检查Object是否设置了ACL。
    -   如果没有设置ACL，那么继续4。
    -   如果设置了ACL，则判断Object的ACL是否允许匿名用户访问。
        -   允许则跳到5。
        -   不允许则拒绝请求，请求结束。
4.  OSS判断Bucket的ACL是否允许匿名用户访问。
    -   允许则继续5。
    -   不允许则返回，请求结束。
5.  权限验证通过，返回Object的内容给用户。

## 带身份验证的请求流程 {#section_ax4_3x2_2gb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4345/15452951201026_zh-CN.png)

1.  用户的请求被发送到OSS的HTTP服务器上。
2.  OSS根据URL解析出Bucket和Object。
3.  OSS根据请求的OSS的AccessKeyId获取请求者的相关身份信息，进行身份鉴权。
    -   如果未获取成功，则返回，请求结束。
    -   如果获取成功，但请求者不被允许访问此资源，则返回，请求结束。
    -   如果获取成功，但OSS端根据请求的HTTP参数计算的签名和请求发送的签名字符串不匹配，则返回，请求结束。
    -   如果身份鉴权成功，那么继续4。
4.  OSS检查Object是否设置了ACL。
    -   如果Object没有设置ACL，那么继续5。
    -   如果Object设置了ACL，OSS判断Object的ACL是否允许此用户访问。
        -   允许则跳到6。
        -   不允许则拒绝请求，请求结束。
5.  OSS判断Bucket的ACL是否允许此用户访问。
    -   允许则继续6。
    -   不允许则返回，请求结束。
6.  权限验证通过，返回Object的内容给用户。

## AccessKey 类型 {#section_en5_23v_tdb .section}

目前访问 OSS 使用的 [AccessKey](intl.zh-CN/开发指南/基本概念介绍.md#section_u3j_nmt_tdb)（AK）有三种类型，具体如下：

-   阿里云账号 AK

    阿里云账号 AK 特指 Bucket 拥有者的 AK，每个阿里云账号提供的 AccessKey 对拥有的资源有完全的权限。每个阿里云账号能够同时拥有不超过5个 active 或者 inactive 的 AK 对（AccessKeyId 和 AccessKeySecret）。

    用户可以登录[AccessKey 管理控制台](https://ak-console.aliyun.com)，申请新增或删除 AK 对。

    每个 AK 对都有active/inactive两种状态。

    -   Active 表明用户的 AK 处于激活状态，可以在身份验证的时候使用。
    -   Inactive 表明用户的 AK 处于非激活状态，不能在身份验证的时候使用。
    **说明：** 请避免直接使用阿里云账号AccessKey。

-   RAM 子账号 AK

    RAM \(Resource Access Management\) 是阿里云提供的资源访问控制服务。RAM 账号 AK 指的是通过 RAM 被授权的 AK。这组 AK 只能按照 RAM 定义的规则去访问Bucket 里的资源。通过 RAM，您可以集中管理您的用户（比如员工、系统或应用程序），以及控制用户可以访问您名下哪些资源的权限。比如能够限制您的用户只拥有对某一个 Bucket 的读权限。子账号是从属于主账号的，并且这些账号下不能拥有实际的任何资源，所有资源都属于主账号。

-   STS 账号 AK

    STS（Security Token Service）是阿里云提供的临时访问凭证服务。STS 账号 AK 指的是通过 STS 颁发的 AK。这组 AK 只能按照 STS 定义的规则去访问 Bucket 里的资源。


## 身份验证具体实现 {#section_qbb_l1f_2gb .section}

目前主要有三种身份验证方式：

-   AK 验证
-   RAM 验证
-   STS 验证

当用户以个人身份向 OSS 发送请求时，其身份验证的实现如下：

1.  用户将发送的请求按照OSS指定的格式生成签名字符串。
2.  用户使用 AccessKeySecret 对签名字符串进行加密产生验证码。
3.  OSS 收到请求以后，通过 AccessKeyId 找到对应的 AccessKeySecret，以同样的方法提取签名字符串和验证码。
    -   如果计算出来的验证码和提供的一样即认为该请求是有效的。
    -   否则，OSS 将拒绝处理这次请求，并返回 HTTP 403错误。

## 带身份验证访问OSS的三种方法 {#section_bpf_5qg_l2b .section}

-   使用控制台访问OSS：控制台中对用户隐藏了身份验证的细节，使用控制台访问OSS的用户无需关注细节。更多信息请参见[下载文件](../../../../intl.zh-CN/控制台用户指南/管理文件/下载文件.md#)。
-   使用SDK访问OSS：OSS提供了多种开发语言的SDK，SDK中实现了签名算法，只需要将AccessKey信息作为参数输入即可。详情请参见各语言SDK文档中的**访问控制**章节，例如 [Java SDK：使用签名URL进行临时授权](../../../../intl.zh-CN/SDK 参考/Java/授权访问.md#ul_nh5_wbd_kfb)、[Python SDK：使用签名URL进行临时授权](../../../../intl.zh-CN/SDK 参考/Python/授权访问.md#section_zx1_55k_kfb)。
-   使用API访问OSS：如果您想用自己喜欢的语言来封装调用RESTful API接口，您需要实现签名算法来计算签名。具体请参见API手册中的[在Header中包含签名](../../../../intl.zh-CN/API 参考/访问控制/在Header中包含签名.md#)和[在URL中包含签名](../../../../intl.zh-CN/API 参考/访问控制/在URL中包含签名.md#)。

