# ACL验证流程 {#concept_xcz_ff5_tdb .concept}

## OSS访问的安全性 {#section_n2b_hf5_tdb .section}

对OSS的HTTP请求可以根据是否携带身份验证信息分为两种请求。一种是带身份验证的请求，一种是不带身份验证的匿名请求。带身份验证的请求有如下两种情况：

-   请求头部中带Authorization，格式为OSS + AccessKeyId + 签名字符串。
-   请求的URL中带OSS AccessKeyId和Signature字段。

## OSS访问验证流程 {#section_f4l_3f5_tdb .section}

**匿名请求的访问流程**

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4345/1532589668959_zh-CN.png)

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

**带身份验证请求的访问流程**

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4345/15325896681026_zh-CN.png)

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

## 带身份验证访问OSS的三种方法 {#section_bpf_5qg_l2b .section}

-   使用控制台访问OSS：控制台中对用户隐藏了身份验证的细节，使用控制台访问OSS的用户无需关注细节。

-   使用SDK访问OSS：OSS提供了多种开发语言的SDK，SDK中实现了签名算法，只需要将AK信息作为参数输入即可。

-   根据API访问OSS：如果您想用自己喜欢的语言来封装调用RESTful API接口，您需要实现签名算法来计算签名。具体的签名算法可以参考API手册中的[在Header中包含签名](../../../../intl.zh-CN/API 参考/访问控制/在Header中包含签名.md#)和[在URL中包含签名](../../../../intl.zh-CN/API 参考/访问控制/在URL中包含签名.md#)。


关于AccessKey相关的解释及更详细的身份验证的操作请参见[访问控制](intl.zh-CN/开发指南/访问与控制/访问控制.md#)。

