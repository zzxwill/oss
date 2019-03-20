# PutBucketACL {#reference_zzr_hk5_tdb .reference}

Modifies the ACL for a bucket. Only the bucket owner can perform this operation.

**Note:** When the bucket owner initiates a PutBucketACL request:

-   The ACL is updated if the bucket already exists and has a different ACL.
-   A bucket with the requested ACL is created if the requested bucket does not exist.

## Request syntax {#section_ds2_2nr_bz .section}

```
PUT /? acl HTTP/1.1
x-oss-acl: Permission
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request header {#section_x2h_1cf_zgb .section}

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|x-oss-acl|String|Yes| Specifies the ACL for the bucket.

 This parameter is included in the PutBucketACL request to set the ACL for the bucket. If this header is not included, the ACL settings do not take effect.

 Valid values: public-read-write, public-read, and private

 |

## Examples {#section_nsl_hnr_bz .section}

**Request example:**

```
PUT /? acl HTTP/1.1
x-oss-acl: public-read
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
```

**Normal response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

**Response example that indicates that the ACL settings do not take effect:**

```
HTTP/1.1 400 Bad Request
x-oss-request-id: 56594298207FB304438516F9
Date: Fri, 24 Feb 2012 03:55:00 GMT
Content-Length: 309
Content-Type: text/xml; charset=UTF-8
Connection: keep-alive
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>InvalidArgument</Code>
  <Message>no such bucket access control exists</Message>
  <RequestId>5***9</RequestId>
  <HostId>***-test.example.com</HostId>
  <ArgumentName>x-oss-acl</ArgumentName>
  <ArgumentValue>error-acl</ArgumentValue>
</Error>
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage a bucket.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Bucket.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Bucket.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Bucket.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Bucket.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage a bucket.md)
-   [Node.js](../../../../../intl.en-US/SDK Reference/Node. js/Manage a bucket.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Manage buckets.md)

## Error code {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|AccessDenied|403| -   Authentication information about the user is not included in the PutBucketACL request.
-   You do not have the permission to initiate a PutBucketACL request. Only the bucket owner can perform this operation.

 |

