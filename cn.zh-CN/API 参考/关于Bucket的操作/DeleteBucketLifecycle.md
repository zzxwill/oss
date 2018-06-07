# DeleteBucketLifecycle {#reference_wl1_xsw_tdb .reference}

通过DeleteBucketLifecycle来删除指定Bucket的生命周期配置。

## 请求语法 {#section_krp_cjw_bz .section}

```
DELETE /?lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_af4_djw_bz .section}

-   本操作会删除指定Bucket的所有的生命周期规则。此后，该Bucket中不会有Object被自动删除。
-   如果Bucket或Lifecycle不存在，返回404 Not Found错误，错误码：NoSuchBucket或NoSuchLifecycle。
-   只有Bucket的拥有者才能删除Bucket的Lifecycle配置。如果试图操作一个不属于你的Bucket，OSS返回403 Forbidden错误，错误码：AccessDenied。

## 示例 {#section_qwg_2jw_bz .section}

**请求示例：**

```
DELETE /?lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com  
Date: Mon, 14 Apr 2014 01:17:35 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:6ZVHOehYzxoC1yxRydPQs/CnMZU=

```

**返回示例：**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Mon, 14 Apr 2014 01:17:35 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS

```

