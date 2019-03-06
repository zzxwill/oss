# DeleteBucketWebsite {#reference_zrl_msw_tdb .reference}

DeleteBucketWebsite接口用于关闭存储空间（Bucket）的静态网站托管模式以及跳转规则。只有Bucket的拥有者才能关闭Bucket的静态网站托管模式。

## 请求语法 {#section_iw2_x3w_bz .section}

```
DELETE /?website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例 {#section_wgs_y3w_bz .section}

**请求示例**

```
DELETE /?website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com 
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:LnM4AZ1OeIduZF5vGFWicOMEkVg=

```

**返回示例**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 05:45:34 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

**完整代码**

```
DELETE /?website HTTP/1.1
Date: Fri, 27 Jul 2018 09:10:52 GMT
Host: test.oss-cn-hangzhou-internal.aliyuncs.com
Authorization: OSS a1nBNgkzzxcQMf8u:qPrKwuMaarA4Tfk1pqTCylFs1jY=
User-Agent: aliyun-sdk-python-test/0.4.0

HTTP/1.1 204 No Content
Server: AliyunOSS
Date: Fri, 27 Jul 2018 09:10:52 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5B5AE19C188DC1CE81DAD7C8
```

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404 Not Found|目标Bucket不存在。|
|AccessDenied|403 Forbidden|没有关闭Bucket静态网站托管模式的权限。只有Bucket的拥有者才能关闭Bucket的静态网站托管模式。|

