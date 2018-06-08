# DeleteBucketLifecycle {#reference_wl1_xsw_tdb .reference}

The DeleteBucketLifecycle interface is used to delete the lifecycle configuration of a specified bucket.

## Request syntax {#section_krp_cjw_bz .section}

```
DELETE /? lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_af4_djw_bz .section}

-   This operation deletes all lifecycle rules of a specified bucket.  After that, no objects are automatically deleted in this bucket.
-   If the bucket or lifecycle does not exist, a 404 not found error is returned. The error code is NoSuchBucket or NoSuchLifecycle.
-   Only the bucket owner can delete the lifecycle configuration of a bucket.  If you try to operate a bucket which does not belong to you, OSS returns the 403  Forbidden error with the error code: AccessDenied.Forbidden error, error code: AccessDenied.

## Examples {#section_qwg_2jw_bz .section}

**Request example:**

```
DELETE /? lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com  
Date: Mon, 14 Apr 2014 01:17:35 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:6ZVHOehYzxoC1yxRydPQs/CnMZU=

```

**Response example:**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Mon, 14 Apr 2014 01:17:35 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS

```

