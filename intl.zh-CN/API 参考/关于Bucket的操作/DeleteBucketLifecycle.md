# DeleteBucketLifecycle {#reference_wl1_xsw_tdb .reference}

DeleteBucketLifecycle接口用于删除指定存储空间（Bucket）的生命周期规则。使用DeleteBucketLifecycle接口删除指定Bucket所有的生命周期规则后，该Bucket中的文件（Object）不会被自动删除。只有Bucket的拥有者才能删除该Bucket的生命周期规则。

## 请求语法 {#section_krp_cjw_bz .section}

```
DELETE /?lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例 {#section_qwg_2jw_bz .section}

**请求示例**

```
DELETE /?lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com  
Date: Mon, 14 Apr 2014 01:17:35 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:6ZVHOehYzxoC1yxRydPQs/CnMZU=

```

**返回示例**

**说明：** 如果删除不存在的Lifecycle，则返回204状态码。

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Mon, 14 Apr 2014 01:17:35 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS

```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../intl.zh-CN/SDK 参考/Java/生命周期.md)
-   [Python](../../../../../intl.zh-CN/SDK 参考/Python/生命周期.md)
-   [PHP](../../../../../intl.zh-CN/SDK 参考/PHP/生命周期.md)
-   [Go](../../../../../intl.zh-CN/SDK 参考/Go/生命周期.md)
-   [C](../../../../../intl.zh-CN/SDK 参考/C/生命周期.md)
-   [.NET](../../../../../intl.zh-CN/SDK 参考/.NET/生命周期.md)
-   [Node.js](../../../../../intl.zh-CN/SDK 参考/Node.js/生命周期.md)
-   [Ruby](../../../../../intl.zh-CN/SDK 参考/Ruby/生命周期.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|AccessDenied|403|没有删除该Bucket生命周期规则的权限。只有Bucket的拥有者才可以删除Bucket的生命周期规则。|

