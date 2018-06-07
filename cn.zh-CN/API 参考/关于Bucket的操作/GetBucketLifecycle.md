# GetBucketLifecycle {#reference_zq5_grw_tdb .reference}

GetBucketLifecycle用于查看Bucket的Lifecycle配置。

```
GET /?lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_vff_13w_bz .section}

-   只有Bucket的拥有者才能查看Bucket的Lifecycle配置，否则返回403 Forbidden错误，错误码：AccessDenied。
-   如果Bucket或Lifecycle不存在，返回404 Not Found错误，错误码：NoSuchBucket或NoSuchLifecycle。

## 示例 {#section_xzz_13w_bz .section}

**请求示例：**

```
Get /?lifecycle HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Mon, 14 Apr 2014 01:17:29 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=

```

**已设置Lifecycle的返回示例：**

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

**未设置Lifecycle的返回示例：**

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

