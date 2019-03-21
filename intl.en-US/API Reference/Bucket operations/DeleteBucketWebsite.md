# DeleteBucketWebsite {#reference_zrl_msw_tdb .reference}

Disables the static website hosting mode and clears the redirection rules for a bucket. Only the owner of a bucket can disable the static website hosting mode for the bucket.

## Request syntax {#section_iw2_x3w_bz .section}

```
DELETE /?website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Examples {#section_wgs_y3w_bz .section}

Request example

```
DELETE /?website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com 
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Authorization: OSS qn6q**************tfjbyc:LnM4AZ1OeIduZF5vGFWicOMEkVg=

```

Response example

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 05:45:34 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

Complete code

```
DELETE /?website HTTP/1.1
Date: Fri, 27 Jul 2018 09:10:52 GMT
Host: test.oss-cn-hangzhou-internal.aliyuncs.com
Authorization: OSS a1nB******cQMf8u:qPrKwuMaarA4Tfk1pqTCylFs1jY=
User-Agent: aliyun-sdk-python-test/0.4.0

HTTP/1.1 204 No Content
Server: AliyunOSS
Date: Fri, 27 Jul 2018 09:10:52 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5B5AE19C188DC1CE81DAD7C8
```

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404 Not Found|The bucket that you want to disable the static website hosting mode for does not exist.|
|AccessDenied|403 Forbidden|You do not have the permission to disable the static website hosting mode for the bucket. Only the owner of a bucket can disable the static website hosting mode for a bucket.|

