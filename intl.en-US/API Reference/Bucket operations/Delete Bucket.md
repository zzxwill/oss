# Delete Bucket {#reference_o1j_rrw_tdb .reference}

DeleteBucket interface is used to delete a bucket.

## Request syntax {#section_ign_23w_bz .section}

```
DELETE / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_t13_f3w_bz .section}

-   If a bucket does not exist, the error “404 no content” is returned. Error code: NoSuchBucket.
-   To prevent the trouble caused by accidental deletion, OSS does not allow the bucket owner to delete a non-empty bucket.
-   If you try to delete a non-empty bucket, the error 409 Conflict with the error code: BucketNotEmpty is returned.
-   Only the bucket owner has the permission to delete the bucket. If you try to delete a bucket you have no permission for, the error 403 Forbidden is returned. Error code: AccessDenied.

## Example {#section_md1_g3w_bz .section}

**Request example:**

```
DELETE / HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:31:04 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=

```

**Response example:**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 05:31:04 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

