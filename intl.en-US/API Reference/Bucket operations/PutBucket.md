# PutBucket {#reference_wdh_fj5_tdb .reference}

Creates a bucket.

**Note:** 

-   Anonymous access is not supported.
-   A user can create a maximum of 30 buckets in a region.
-   Each region has an endpoint. For more information, see [Regions and endpoints](../../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).

## Request syntax {#section_w34_mmr_bz .section}

```
PUT / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
x-oss-acl: Permission
Authorization: SignatureValue
<? xml version="1.0" encoding="UTF-8"? >
<CreateBucketConfiguration>
    <StorageClass>Standard</StorageClass>
</CreateBucketConfiguration>
```

## Request header {#section_asb_zxq_3gb .section}

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|x-oss-acl|String|No|Specifies the ACL for the bucket.Valid values: public-read-write, public-read, and private

**Note:** If you do not set an ACL for a bucket when creating it, the ACL for the bucket is set to private by default.

|

## Request element {#section_mtf_lwq_3gb .section}

|Element|Type|Description|
|-------|----|-----------|
|StorageClass|String|Specifies the storage class of the bucket.Valid values: Standard, IA, and Archive.

|

## Examples {#section_axr_rmr_bz .section}

Request example:

```
PUT / HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2017 03:15:40 GMT
x-oss-acl: private
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:77Dvh5wQgIjWjwO/KyRt8dOPfo8=
<? xml version="1.0" encoding="UTF-8"? >
<CreateBucketConfiguration>
    <StorageClass>Standard</StorageClass>
</CreateBucketConfiguration>
```

Response example:

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2017 03:15:40 GMT
Location: /oss-example
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK {#section_egl_m2c_5gb .section}

SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage a bucket.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Bucket.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Bucket.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Bucket.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Bucket.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage a bucket.md)
-   [Android](../../../../../intl.en-US/SDK Reference/Android/Manage buckets.md)
-   [iOS](../../../../../intl.en-US/SDK Reference/iOS/Manage buckets.md)
-   [Node.js](../../../../../intl.en-US/SDK Reference/Node. js/Manage a bucket.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Manage buckets.md)

## Error code {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidBucketName|400|The bucket name does not conform to the naming convention.|
|AccessDenied|403| -   Authentication information is not carried in a PutBucket request
-   You are not authorized to perform operations on the bucket.

 |
|TooManyBuckets|400|More than 30 buckets are created within a region.|

