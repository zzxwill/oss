# DeleteBucketWebsite {#reference_zrl_msw_tdb .reference}

DeleteBucketWebsite is used to disable the static website hosting mode and the redirection rules for a bucket.

## Request syntax {#section_iw2_x3w_bz .section}

```
DELETE /? website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_dzz_x3w_bz .section}

-   If the bucket does not exist, the error “404 no content” is returned. Error code: NoSuchBucket.
-   Only the bucket owner can disable the static website hosting mode for a bucket. If you try to operate a bucket which is not owned by you, OSS returns the "403 Forbidden" error. Error code: AccessDenied.

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

**Complete code:**

```
DELETE /? website HTTP/1.1
Date: Fri, 27 Jul 2018 09:10:52 GMT
Host: test.oss-cn-hangzhou-internal.aliyuncs.com
Authorization: OSS a1nBNgkzzxcQMf8u:qPrKwuMaarA4Tfk1pqTCylFs1jY=
User-Agent: aliyun-sdk-python-test/0.4.0

HTTP/1.1 204 No Content
Server: AliyunOSS
Date: Fri, 27 Jul 2018 09:10:52 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5B5AE19C188DC1CE81DAD7C8
```

