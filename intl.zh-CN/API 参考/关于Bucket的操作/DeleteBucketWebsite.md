# DeleteBucketWebsite {#reference_zrl_msw_tdb .reference}

DeleteBucketWebsite操作用于关闭bucket的静态网站托管模式。

## 请求语法 {#section_iw2_x3w_bz .section}

```
DELETE /?website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_dzz_x3w_bz .section}

-   如果Bucket不存在，返回404 no content错误，错误码：NoSuchBucket。
-   只有Bucket的拥有者才能关闭Bucket的静态网站托管模式。如果试图操作一个不属于你的Bucket，OSS返回403 Forbidden错误，错误码：AccessDenied。

## 示例 {#section_wgs_y3w_bz .section}

**请求示例：**

```
DELETE /?website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com 
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:LnM4AZ1OeIduZF5vGFWicOMEkVg=

```

**返回示例：**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 05:45:34 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

