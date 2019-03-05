# DeleteBucket {#reference_o1j_rrw_tdb .reference}

DeleteBucket用于删除某个存储空间（Bucket）。

**说明：** 

-   只有Bucket的拥有者才有权限删除该Bucket。
-   为了防止误删除的发生，OSS不允许删除一个非空的Bucket。

## 请求语法 {#section_ign_23w_bz .section}

```
DELETE / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例 {#section_md1_g3w_bz .section}

**正常删除**

请求示例

```
DELETE / HTTP/1.1
Host: test.oss-cn-hangzhou.aliyuncs.com
Accept-Encoding: identity
User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
Accept: */*
Connection: keep-alive
date: Tue, 15 Jan 2019 08:19:04 GMT
authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=
Content-Length: 0
```

返回示例

```
HTTP/1.1 204 No Content
Server: AliyunOSS
Date: Tue, 15 Jan 2019 08:19:04 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5C3D9778CC1C2AEDF85BD9B7
x-oss-server-time: 190
```

**要删除的Bucket不存在**

请求示例

```
DELETE / HTTP/1.1
Host: test.oss-cn-hangzhou.aliyuncs.com
Accept-Encoding: identity
User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
Accept: */*
Connection: keep-alive
date: Tue, 15 Jan 2019 07:53:24 GMT
authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=
Content-Length: 0
```

返回示例

```
HTTP/1.1 404 Not Found
Server: AliyunOSS
Date: Tue, 15 Jan 2019 07:53:25 GMT
Content-Type: application/xml
Content-Length: 288
Connection: keep-alive
x-oss-request-id: 5C3D9175B6FC201293AD4890

<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>NoSuchBucket</Code>
  <Message>The specified bucket does not exist.</Message>
  <RequestId>5C3D9175B6FC201293AD4890</RequestId>
  <HostId>test.oss-cn-hangzhou.aliyuncs.com</HostId>
  <BucketName>test</BucketName>
</Error>
```

**要删除的Bucket非空**

请求示例

```
DELETE / HTTP/1.1
Host: test.oss-cn-hangzhou.aliyuncs.com
Accept-Encoding: identity
User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
Accept: */*
Connection: keep-alive
date: Tue, 15 Jan 2019 07:35:06 GMT
authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=
Content-Length: 0
```

返回示例

```
HTTP/1.1 409 Conflict
Server: AliyunOSS
Date: Tue, 15 Jan 2019 07:35:06 GMT
Content-Type: application/xml
Content-Length: 296
Connection: keep-alive
x-oss-request-id: 5C3D8D2A0ACA54D87B43C048
x-oss-server-time: 16

<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>BucketNotEmpty</Code>
  <Message>The bucket you tried to delete is not empty.</Message>
  <RequestId>5C3D8D2A0ACA54D87B43C048</RequestId>
  <HostId>test.oss-cn-hangzhou.aliyuncs.com</HostId>
  <BucketName>test</BucketName>
</Error>
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../intl.zh-CN/SDK 参考/Java/存储空间.md)
-   [Python](../../../../../intl.zh-CN/SDK 参考/Python/存储空间.md)
-   [PHP](../../../../../intl.zh-CN/SDK 参考/PHP/存储空间.md)
-   [Go](../../../../../intl.zh-CN/SDK 参考/Go/存储空间.md)
-   [C](../../../../../intl.zh-CN/SDK 参考/C/存储空间.md)
-   [.NET](../../../../../intl.zh-CN/SDK 参考/.NET/存储空间.md)
-   [iOS](../../../../../intl.zh-CN/SDK 参考/iOS/存储空间.md)
-   [Node.js](../../../../../intl.zh-CN/SDK 参考/Node.js/存储空间.md)
-   [Ruby](../../../../../intl.zh-CN/SDK 参考/Ruby/存储空间.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|AccessDenied|403 Forbidden|没有删除该Bucket的权限。只有Bucket的拥有者才能删除该Bucket。|

