# DeleteBucketLogging {#reference_jrn_gsw_tdb .reference}

DeleteBucketLogging interface is used to disable the access logging function of a bucket.

## Request syntax {#section_lyn_m3w_bz .section}

```
DELETE /? logging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_anj_n3w_bz .section}

-   If the bucket does not exist, Error 404 No Content with the error code "NoSuchBucket" is returned.
-   Only the bucket owner can disable the access logging function for the bucket. If you try to operate a bucket which does not belong to you, OSS returns the error 403  Forbidden with the error code: AccessDenied.
-   If the access logging function is not enabled for the target bucket, HTTP status code 204 is returned.

## Example {#section_isc_43w_bz .section}

**Request example:**

```
DELETE /? logging HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:35:24 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:6ZVHOehYzxoC1yxRydPQs/CnMZU=

```

**Return example:**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 05:35:24 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

