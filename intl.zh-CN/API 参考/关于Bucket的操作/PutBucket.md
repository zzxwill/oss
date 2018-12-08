# PutBucket {#reference_wdh_fj5_tdb .reference}

PutBucket接口用于创建 Bucket（不支持匿名访问）。

创建的 Bucket 所在的 Region 和发送请求的 Endpoint 所对应的 Region 一致。Bucket所在的数据中心确定后，该Bucket中的所有Object将一直存放在对应Region。详情请参见[访问域名和数据中心](../../../../intl.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#) 。

## 请求语法 {#section_w34_mmr_bz .section}

```
PUT / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
x-oss-acl: Permission
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration>
    <StorageClass>Standard</StorageClass>
</CreateBucketConfiguration>
```

## 细节分析 {#section_yrk_pmr_bz .section}

-   可以通过 Put 请求中的 `x-oss-acl`header 来设置Bucket访问权限。目前Bucket有三种访问权限：public-read-write，public-read 和 private。
-   如果创建的 Bucket 没有指定访问权限，则默认使用 Private 权限。
-   如果创建的 Bucket 不符合命名规范，则返回400 Bad Request 消息。错误码：InvalidBucketName。
-   如果用户发起 PUT Bucket 请求时没有传入用户验证信息，则返回403 Forbidden消息。错误码：AccessDenied。
-    同一用户在同一 Region 内最多可创建30 个Bucket。如果超过30 个，则返回400 Bad Request 消息。错误码：TooManyBuckets。
-   创建 Bucket，可以指定 Bucket 的存储类型，可选值为Standard、IA和Archive。
-   创建 Bucket，可以指定 Bucket 的数据容灾类型（DataRedundancyType），可选值为LRS（本地容灾类型，默认值）、ZRS（同城容灾类型）。

## 示例 {#section_axr_rmr_bz .section}

请求示例：

```
PUT / HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2017 03:15:40 GMT
x-oss-acl: private
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:77Dvh5wQgIjWjwO/KyRt8dOPfo8=
<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration>
    <StorageClass>Standard</StorageClass>
</CreateBucketConfiguration>
```

返回示例：

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2017 03:15:40 GMT
Location: /oss-example
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

