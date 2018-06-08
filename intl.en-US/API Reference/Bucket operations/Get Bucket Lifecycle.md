# Get Bucket Lifecycle {#reference_zq5_grw_tdb .reference}

GetBucketLifecycle is used to view the lifecycle configuration of a bucket.

```
GET /? lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_vff_13w_bz .section}

-   Only the owner of a bucket can view the lifecycle configuration of the bucket. If other users attempt to access the configuration, the error 403 Forbidden with the error code: AccessDenied is returned.
-   If the bucket or lifecycle does not exist, the error 404 Not Found with the error code: NoSuchBucket or NoSuchLifecycle is returned.

## Example {#section_xzz_13w_bz .section}

**Request example:**

```
Get /? lifecycle HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Mon, 14 Apr 2014 01:17:29 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=

```

**Response example with bucket lifecycle configured:**

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

**Response example with no bucket lifecycle configured:**

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

