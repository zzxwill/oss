# DeleteBucketcors {#reference_fjn_szc_xdb .reference}

DeleteBucketcors用于关闭指定Bucket对应的CORS功能并清空所有规则。

## 请求语法 {#section_iv1_wzc_xdb .section}

```
DELETE /?cors HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_wlr_xzc_xdb .section}

-   如果Bucket不存在，返回404 no content错误，错误码：NoSuchBucket。
-   只有Bucket的拥有者才能删除Bucket对应的CORS规则。如果试图操作一个不属于你的Bucket，OSS返回403 Forbidden错误，错误码：AccessDenied。

## 示例 {#section_i1t_zzc_xdb .section}

**请求示例：**

```
DELETE /?cors HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:LnM4AZ1OeIduZF5vGFWicOMEkVg=
```

**返回示例：**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 5051845BC4689A033D0022BC
Date: Fri, 24 Feb 2012 05:45:34 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

