# DeleteBucketcors {#reference_fjn_szc_xdb .reference}

DeleteBucketcors is used to turn off the CORS function for the specified bucket and clear all rules.

## Request syntax {#section_iv1_wzc_xdb .section}

```
DELETE /? cors HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_wlr_xzc_xdb .section}

-   If the bucket does not exist, OSS returns the 404 no content error, error code: NoSuchBucket.
-   Only the owner of the bucket can delete the CORS rules corresponding to the bucket. If you try to operate a bucket which does not belong to you, OSS returns the 403  Forbidden error, error code: accessdenied.

## Examples {#section_i1t_zzc_xdb .section}

**Request example:**

```
DELETE /? cors HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:LnM4AZ1OeIduZF5vGFWicOMEkVg=
```

**Response example:**

```
HTTP/1.1 204 No Content 
x-oss-request-id: 5051845BC4689A033D0022BC
Date: Fri, 24 Feb 2012 05:45:34 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

