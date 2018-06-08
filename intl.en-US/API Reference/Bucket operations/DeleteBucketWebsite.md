# DeleteBucketWebsite {#reference_zrl_msw_tdb .reference}

The  DeleteBucketWebsite is used to disable the static website hosting mode of a bucket.

## Request syntax {#section_iw2_x3w_bz .section}

```
DELETE /? website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_dzz_x3w_bz .section}

-   If the bucket does not exist, the error 404 No Content with the error code: NoSuchBucket is returned.
-   Only the bucket owner can disable the bucket’s static website hosting mode.  If you try to operate a bucket which does not belong to you, OSS returns the error 403  Forbidden with the error code: AccessDenied.

## Example {#section_wgs_y3w_bz .section}

**Request example:**

```
DELETE /? website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com 
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:LnM4AZ1OeIduZF5vGFWicOMEkVg=

```

**Response example:**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 05:45:34 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

