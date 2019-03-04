# DeleteBucketLogging {#reference_jrn_gsw_tdb .reference}

DeleteBucketLogging用于关闭存储空间（Bucket）的访问日志记录功能。只有Bucket的拥有者才有权限关闭Bucket访问日志记录功能。

## 请求语法 {#section_lyn_m3w_bz .section}

```
DELETE /?logging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例 {#section_isc_43w_bz .section}

**请求示例**

```
DELETE /?logging HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:35:24 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:6ZVHOehYzxoC1yxRydPQs/CnMZU=

```

**返回示例**

**说明：** 如果目标Bucket没有开启访问日志记录功能，则返回204状态码。

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 05:35:24 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/访问日志.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/访问日志.md)
-   [PHP](../../../../../cn.zh-CN/SDK 参考/PHP/访问日志.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/设置访问日志.md)
-   [C](../../../../../cn.zh-CN/SDK 参考/C/访问日志.md)
-   [.NET](../../../../../cn.zh-CN/SDK 参考/.NET/访问日志.md)
-   [Node.js](../../../../../cn.zh-CN/SDK 参考/Node.js/访问日志.md)
-   [Ruby](../../../../../cn.zh-CN/SDK 参考/Ruby/设置访问日志.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|AccessDenied|403|没有关闭Bucket访问日志记录功能的权限。只有Bucket的拥有者才可以关闭Bucket访问日志记录功能。|

