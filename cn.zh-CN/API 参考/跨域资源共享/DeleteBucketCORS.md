# DeleteBucketCORS {#reference_fjn_szc_xdb .reference}

DeleteBucketcors用于关闭指定存储空间（Bucket）对应的跨域资源共享（CORS）功能并清空所有规则。

## 请求语法 {#section_iv1_wzc_xdb .section}

```
DELETE /?cors HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例 {#section_i1t_zzc_xdb .section}

**请求示例**

```
DELETE /?cors HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:LnM4AZ1OeIduZF5vGFWicOMEkVg=
```

**返回示例**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 5051845BC4689A033D0022BC
Date: Fri, 24 Feb 2012 05:45:34 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

## 错误码 {#section_nfp_nfc_5gb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|AccessDenied|403|没有删除权限。只有Bucket的拥有者才有权限删除Bucket对应的CORS规则。|

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/跨域资源共享.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/跨域资源共享.md)
-   [PHP](../../../../../cn.zh-CN/SDK 参考/PHP/跨域资源共享.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/跨域资源共享.md)

