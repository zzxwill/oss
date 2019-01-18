# DeleteBucketLogging {#reference_jrn_gsw_tdb .reference}

DeleteBucketLogging用于关闭 Bucket 访问日志记录功能。

## 请求语法 {#section_lyn_m3w_bz .section}

```
DELETE /?logging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_anj_n3w_bz .section}

-   如果 Bucket 不存在，返回 404 Not Found 错误，错误码：NoSuchBucket。
-   只有 Bucket 的拥有者才能关闭 Bucket 访问日志记录功能。非Bucket拥有者操作该Bucket，OSS 返回 403 Forbidden 错误，错误码：AccessDenied。
-   如果目标 Bucket 没有开启 Logging 日志记录功能，返回 HTTP 状态码 204。

## 示例 {#section_isc_43w_bz .section}

**请求示例**

```
DELETE /?logging HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:35:24 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:6ZVHOehYzxoC1yxRydPQs/CnMZU=

```

**返回示例**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 05:35:24 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

