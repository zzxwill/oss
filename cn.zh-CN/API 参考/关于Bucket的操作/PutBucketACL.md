# PutBucketACL {#reference_zzr_hk5_tdb .reference}

PutBucketACL接口用于修改存储空间（Bucket）的访问权限。

目前Bucket有三种访问权限：public-read-write，public-read和private。PutBucketACL操作通过Put请求中的 x-oss-acl Header来设置权限，如果没有该Header，权限设置不生效。这个操作只有该Bucket的创建者有权限执行。如果操作成功，则返回200 OK；否则返回相应的错误信息和错误码。

## 请求语法 {#section_ds2_2nr_bz .section}

```
PUT /?acl HTTP/1.1
x-oss-acl: Permission
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_l4x_fnr_bz .section}

-   Bucket拥有者发起PutBucketACL请求时，如果Bucket已存在但权限不一致，将更新权限。如果Bucket不存在，将创建Bucket。
-   发起PutBucketACL请求时，如果没有传入用户验证信息，则返回403 Forbidden消息。错误码：AccessDenied。

## 示例 {#section_nsl_hnr_bz .section}

请求示例：

```
PUT /?acl HTTP/1.1
x-oss-acl: public-read
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
```

正常的返回示例：

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

设置权限无效的返回示例：

```
HTTP/1.1 400 Bad Request
x-oss-request-id: 56594298207FB304438516F9
Date: Fri, 24 Feb 2012 03:55:00 GMT
Content-Length: 309
Content-Type: text/xml; charset=UTF-8
Connection: keep-alive
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>InvalidArgument</Code>
  <Message>no such bucket access control exists</Message>
  <RequestId>5***9</RequestId>
  <HostId>***-test.example.com</HostId>
  <ArgumentName>x-oss-acl</ArgumentName>
  <ArgumentValue>error-acl</ArgumentValue>
</Error>
```

