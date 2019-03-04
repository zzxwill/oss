# GetBucketLifecycle {#reference_zq5_grw_tdb .reference}

GetBucketLifecycle接口用于查看存储空间（Bucket）的生命周期规则（Lifecycle）。只有Bucket的拥有者才有权限查看Bucket的生命周期规则。

## 请求语法 {#section_kn3_zhw_bz .section}

```
GET /?lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例 {#section_xzz_13w_bz .section}

**请求示例**

```
Get /?lifecycle HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Mon, 14 Apr 2014 01:17:29 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=

```

**返回示例（已设置生命周期规则）**

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Mon, 14 Apr 2014 01:17:29 GMT
Connection: keep-alive
Content-Length: 255
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<LifecycleConfiguration>
  <Rule>
    <ID>delete after one day</ID>
    <Prefix>logs/</Prefix>
    <Status>Enabled</Status>
    <Expiration>
      <Days>1</Days>
    </Expiration>
  </Rule>
</LifecycleConfiguration>

```

**返回示例（未设置生命周期规则）**

```
HTTP/1.1 404
x-oss-request-id: 534B371674E88A4D8906008B
Date: Mon, 14 Apr 2014 01:17:29 GMT
Connection: keep-alive
Content-Length: 278
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <BucketName>oss-example</BucketName>
  <Code>NoSuchLifecycle</Code>
  <Message>No Row found in Lifecycle Table.</Message>
  <RequestId>534B372974E88A4D89060099</RequestId>
  <HostId> BucketName.oss.example.com</HostId>
</Error>
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/生命周期.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/生命周期.md)
-   [PHP](../../../../../cn.zh-CN/SDK 参考/PHP/生命周期.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/生命周期.md)
-   [C](../../../../../cn.zh-CN/SDK 参考/C/生命周期.md)
-   [.NET](../../../../../cn.zh-CN/SDK 参考/.NET/生命周期.md)
-   [Node.js](../../../../../cn.zh-CN/SDK 参考/Node.js/生命周期.md)
-   [Ruby](../../../../../cn.zh-CN/SDK 参考/Ruby/生命周期.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|AccessDenied|403 Forbidden|没有权限查看Bucket的生命周期规则。只有Bucket的拥有者才能查看Bucket的生命周期规则。|
|NoSuchBucket或NoSuchLifecycle|404 Not Found|Bucket不存在或没有配置生命周期规则。|

