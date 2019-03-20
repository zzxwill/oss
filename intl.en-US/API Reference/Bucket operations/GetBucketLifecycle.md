# GetBucketLifecycle {#reference_zq5_grw_tdb .reference}

Views the lifecycle rules for a bucket. Only the owner of a bucket can view the lifecycle rules for the bucket.

## Request syntax {#section_kn3_zhw_bz .section}

```
GET /? lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Examples {#section_xzz_13w_bz .section}

Request example:

```
Get /? lifecycle HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Mon, 14 Apr 2014 01:17:29 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=

```

Response example returned when lifecycle rules are configured for the bucket:

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Mon, 14 Apr 2014 01:17:29 GMT
Connection: keep-alive
Content-Length: 255
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
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

Response example returned when no bucket lifecycle rules are configured for the bucket:

```
HTTP/1.1 404
x-oss-request-id: 534B371674E88A4D8906008B
Date: Mon, 14 Apr 2014 01:17:29 GMT
Connection: keep-alive
Content-Length: 278
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <BucketName>oss-example</BucketName>
  <Code>NoSuchLifecycle</Code>
  <Message>No Row found in Lifecycle Table.</Message>
  <RequestId>534B372974E88A4D89060099</RequestId>
  <HostId> BucketName.oss.example.com</HostId>
</Error>
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage lifecycle rules.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Manage lifecycle rules.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Manage lifecycle rules.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Manage lifecycle rules.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Manage lifecycle rules.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage lifecycle rules.md)
-   [Node.js](../../../../../intl.en-US/SDK Reference/Node. js/Manage lifecycle rules.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Manage lifecycle rules.md)

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|AccessDenied|403 Forbidden|You do not have the permission to view the lifecycle rules for the bucket. Only the owner of a bucket can view the lifecycle rules for the bucket.|
|NoSuchBucket or NoSuchLifecycle|404 Not Found|The bucket does not exist or no lifecycle rules are configured for the bucket.|

