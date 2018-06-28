# PutBucket {#reference_wdh_fj5_tdb .reference}

The PutBucket interface is used to create a bucket \(anonymous access is not supported \).

The region of the created bucket is consistent with the region of the endpoint from which the request is sent. Once the data center of the bucket is determined, all objects in this bucket are stored in the corresponding region. For more information, see [Regions and endpoints](../../../../intl.en-US/Developer Guide/Regions and endpoints.md#) .

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

## Detail analysis {#section_yrk_pmr_bz .section}

-   You can use the `x-oss-acl` header in a Put request to set access permissions for a bucket.   Currently, three access permissions are available for a bucket: public-read-write, public-read, and private.
-   If the requested bucket already exists, 409 Conflict is returned. Error code: BucketAlreadyExists.
-   If the bucket to be created does not conform to the naming conventions, the message of 400 Bad Request is returned. Error code: InvalidBucketName.
-   If the information for user authentication is not introduced when you initiate a Put Bucket request, the message of 403 Forbidden is returned. Error code: AccessDenied.
-   If the maximum number of buckets allowed to be created \(30 by default\) exceeds when you initiate a PutBucket request, the message of 400 Bad Request is returned. Error code: TooManyBuckets.
-   If no access permission is specified for the created bucket, the `Private` permission applies by default.
-   The storage type of a new bucket can be specified. Standard, IA, and Archive are available.

## Example {#section_axr_rmr_bz .section}

**Request example:**

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

**Response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2017 03:15:40 GMT
Location: /oss-example
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

