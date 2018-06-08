# OSS 权限问题及排查 {#concept_jfh_4s3_wdb .concept}

## OSS 403问题 {#section_llv_4s3_wdb .section}

OSS 403指OSS返回的HTTP状态码是403，可以简单的理解为没有权限访问，服务器收到请求但拒绝提供服务。OSS 403错误及原因如下表：

|错误|错误码错误信息|错误原因|解决办法|
|:-|:------|:---|:---|
|SignatureDoesNotMatch|ErrorCode: SignatureDoesNotMatchErrorMessage: The request signature we calculated does not match the signature you provided. Check your key and signing method.|客户端和服务计算的签名不符|[OSS 403错误及排查](cn.zh-CN/常见错误排除/OSS 403错误及排查.md#)|
|PostObject|ErrorCode: AccessDeniedErrorMessage: Invalid according to Policy: Policy expired.ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy Condition failed: …|PostObject中Policy无效|[PostObject](../cn.zh-CN/API 参考/关于Object操作/PostObject.md#)|
|Cors|ErrorCode: AccessForbiddenErrorMessage: CORSResponse: This CORS request is not allowed. This is usually because the evalution of Origin, request method / Access-Control-Request-Method or Access-Control-Requet-Headers are not whitelisted by the resource’s CORS spec.|CORS没有配置或配置不对|[OSS设置跨域访问](../cn.zh-CN/控制台用户指南/管理存储空间/设置跨域访问.md#)|
|Refers|ErrorCode: AccessDeniedErrorMessage: You are denied by bucket referer policy.|请检查Bucket的Referer配置|[OSS防盗链](../cn.zh-CN/控制台用户指南/管理存储空间/设置防盗链.md#)|
|AccessDenied|见以下权限常见错误|无权限|下面详细讲述|

其中，权限问题是403错误的一部分。权限问题的错误是`AccessDenied`。以下将详细讲述这类错误。

## 权限常见错误 {#section_ttw_rs3_wdb .section}

权限问题是当前用户没有指定操作的权限。OSS返回的错误及原因见下表：

|序号|错误|原因|
|:-|:-|:-|
|1|ErrorCode: AccessDeniedErrorMessage: The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint.|Bucket和Endpoint不符|
|2|ErrorCode: AccessDeniedErrorMessage: You are forbidden to list buckets.|无listBuckets权限|
|3|ErrorCode: AccessDeniedErrorMessage: You do not have write acl permission on this object|无setObjectAcl权限|
|4|ErrorCode: AccessDeniedErrorMessage: You do not have read acl permission on this object.|无getObjectAcl权限|
|5|ErrorCode: AccessDeniedErrorMessage: The bucket you visit is not belong to you.|子用户没有Bucket管理的权限（如getBucketAcl CreateBucket、deleteBucket setBucketReferer、 getBucketReferer等）|
|6|ErrorCode: AccessDeniedErrorMessage: You have no right to access this object because of bucket acl.|子用户/临时用户没有访问Object的权限（如putObject getObject、appendObject deleteObject、postObject）等|
|7|ErrorCode: AccessDeniedErrorMessage: Access denied by authorizer’s policy.|临时用户访问无权限，该临时用户角色扮演指定授权策略，该授权策略无权限|
|8|ErrorCode: AccessDeniedErrorMessage: You have no right to access this object.|子用户/临时用户无当前操作权限（如initiateMultipartUpload等）|

## 权限问题排查 {#section_usf_5s3_wdb .section}

辨别密钥是主用户、子用户还是临时用户的。

-   是否是主用户的密钥：

    需要到 [控制台](https://ak-console.aliyun.com) 查看AccessKeyID是否存在，如果存在说明是主用户。

-   查看子用户的权限，即该子用户的授权策略：

    在控制台**访问控制** \> **用户管理** \> **管理** \> **用户详情** \> **用户AccessKey**，查看子用户的AccessKeyID，并找到对应的子用户；在控制台**访问控制** \> **用户管理** \> **管理** \> **用户授权策略** \> **个人授权策略/加入组的授权策略** \> **查看子账户的权限**

    。

-   查看临时用户的权限，即对应角色的权限：

    临时用户的密钥的AccessKeyID，以STS开头比较好辨认，如“STS.MpsSonrqGM8bGjR6CRKNMoHXe”。 在控制台**访问控制** \> **角色管理** \> **管理** \> **角色授权策略** \> **查看权限**

    。


访问权限错误流程如下图：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4908/3008_zh-CN.png)

权限检查流程如下：

1.  列出需要的权限和资源。
2.  检查Action是否有需要的操作。
3.  Resource是否是需要的操作对象。
4.  Effect是否是Allow而不是Deny。
5.  Condition是否正确。

如果检查无法发现错误，需要调试步骤如下：

1.  如果有Condition的话先将其去掉。
2.  Effect中去除Deny。
3.  Resource换成”Resource”: “\*”。
4.  Action换成”Action”: “oss:\*”。

**说明：** 

-   权限策略的生成推荐使用OSS授权策略生成工具 [RAM Policy Editor](http://gosspublic.alicdn.com/ram-policy-editor/index.html?spm=a2c4g.11186623.2.11.xeubSy)。
-   如果想更多了解阿里云访问控制（RAM），请参见 [阿里云访问控制初探](https://yq.aliyun.com/articles/57895?spm=a2c4g.11186623.2.12.xeubSy) 。

