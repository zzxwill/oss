# DeleteBucketLifecycle {#reference_wl1_xsw_tdb .reference}

通过DeleteBucketLifecycle接口来删除指定 Bucket 的生命周期规则。

## 请求语法 {#section_krp_cjw_bz .section}

```
DELETE /?lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_af4_djw_bz .section}

-   DeleteBucketLifecycle 操作会删除指定 Bucket 所有的生命周期规则。此后，该 Bucket 中不会有 Object 被自动删除。
-   如果 Bucket 不存在，返回 404 Not Found 错误，错误码：NoSuchBucket。
-   如果删除不存在的 Lifecycle，返回 204 状态码。
-   只有 Bucket 的拥有者才能删除该 Bucket 的生命周期规则。非 Bucket 拥有者操作该 Bucket，OSS 返回 403 Forbidden 错误，错误码：AccessDenied。

## 示例 {#section_qwg_2jw_bz .section}

请求示例：

```
DELETE /?lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com  
Date: Mon, 14 Apr 2014 01:17:35 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:6ZVHOehYzxoC1yxRydPQs/CnMZU=

```

返回示例：

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Mon, 14 Apr 2014 01:17:35 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS

```

