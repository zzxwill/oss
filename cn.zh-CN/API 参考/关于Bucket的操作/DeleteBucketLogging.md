# DeleteBucketLogging {#reference_jrn_gsw_tdb .reference}

DeleteBucketLogging接口用于关闭bucket访问日志记录功能。

## 请求语法 {#section_lyn_m3w_bz .section}

```
DELETE /?logging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_anj_n3w_bz .section}

-   如果Bucket不存在，返回404 no content错误，错误码：NoSuchBucket。
-   只有Bucket的拥有者才能关闭Bucket访问日志记录功能。如果试图操作一个不属于你的Bucket，OSS返回403 Forbidden错误，错误码：AccessDenied。
-   如果目标Bucket并没有开启Logging功能，仍然返回HTTP状态码 204。

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

