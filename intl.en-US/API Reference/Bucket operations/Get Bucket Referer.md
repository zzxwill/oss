# Get Bucket Referer {#reference_bs5_rpw_tdb .reference}

GetBucketReferer operation is used to view the referer configuration of a bucket.

## Request syntax {#section_j24_4hw_bz .section}

```
GET /? referer HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response  {#section_jzk_phw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|RefererConfiguration|Container|The container that saves the Referer configuration content. Sub-nodes: AllowEmptyReferer node and RefererList node 

Parent node: none

|
|AllowEmptyReferer|enumerative string|Specify whether the access request with the referer field being null is allowed.  Valid value: true or false-   Default value: true

Parent node: RefererConfiguration

|
|RefererList|container |The container that saves the referer access whitelist.  Parent node: RefererConfiguration 

Sub-node: Referer

|
|Referer|String|Specify a referer access whitelist.  Parent node: RefererList

|

## Detail analysis {#section_gvp_rhw_bz .section}

-   If the bucket does not exist, error 404 is returned. Error code: NoSuchBucket.
-   Only the owner of a bucket can view the referer configuration of the bucket. If other users attempt to access the configuration, the error 403 Forbidden with the error code: AccessDenied is returned.
-   If no referer configuration has been conducted for the bucket, OSS returns the default AllowEmptyReferer value and an empty RefererList.

## Example {#section_hr3_shw_bz .section}

**Request example:**

```
Get /? referer HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Thu, 13 Sep 2012 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=
```

**Response example with a referer rule configured for the bucket:**

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu, 13 Sep 2012 07:51:28 GMT
Connection: keep-alive
Content-Length: 218  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<RefererConfiguration>
<Allowemptyreferer> true </allowemptyreferer>
    <RefererList>
        <Referer> http://www.aliyun.com</Referer>
        <Referer> https://www.aliyun.com</Referer>
        <Referer> http://www. *.com</Referer>
        <Referer> https://www.?.aliyuncs.com</Referer>
    </RefererList>
</RefererConfiguration>
```

**Response example with no referer rule configured for the bucket:**

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu, 13 Sep 2012 07:56:46 GMT
Connection: keep-alive
Content-Length: 308  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<RefererConfiguration>
<AllowEmptyReferer>true</AllowEmptyReferer >
< RefererList />
</RefererConfiguration>
```

