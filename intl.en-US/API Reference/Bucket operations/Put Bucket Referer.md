# Put Bucket Referer {#reference_prc_ys5_tdb .reference}

With the Put Bucket Refereroperation, you can set the referer access whitelist of a bucket and whether the access request with the referer field being null is allowed. 

## Request syntax {#section_yfn_hcw_bz .section}

```
PUT /? referer HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue

<? xml version="1.0" encoding="UTF-8"? >
<RefererConfiguration>
<AllowEmptyReferer>true</AllowEmptyReferer >
    <RefererList>
        <Referer> http://www.aliyun.com</Referer>
        <Referer> https://www.aliyun.com</Referer>
        <Referer> http://www. *.com</Referer>
        <Referer> https://www.?.aliyuncs.com</Referer>
    </RefererList>
</RefererConfiguration>
```

## Request elements {#section_ujq_jcw_bz .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|RefererConfiguration|container|Yes|The container that saves the Referer configuration content Sub-nodes: AllowEmptyReferer node and RefererList node 

Parent node: none

|
|AllowEmptyReferer|enumerative string |Yes|Specify whether the access request with the referer field being null is allowed. Valid value: true or false-   Default value:true 
-   false

Parent node: RefererConfiguration 

 |
|RefererList|container|Yes|The container that saves the referer access whitelist. Parent node: RefererConfiguration 

Sub-node: Referer

 |
|Referer|string|No|Specify a referer access whitelist. Parent node: RefererList

|

## Detail analysis {#section_fcy_lcw_bz .section}

-   Only the bucket owner can initiate a Put Bucket Referer request. Otherwise, the message of 403 Forbidden is returned.  Error code: AccessDenied.
-   The configuration specified in AllowEmptyReferer replaces the previous AllowEmptyReferer configuration. This field is required. By default, AllowEmptyReferer in the system is configured as true.
-   This operation overwrites the previously configured whitelist with the whitelist in the RefererList. When the user-uploaded RefererList is empty \(containing no referer request element\), this operation overwrites the configured whitelist, that is, the previously configured RefererList is deleted.
-   If you have uploaded the Content-MD5 request header, OSS calculates the body’s Content-MD5 and checks if the two are the same. If the two are different, the error code: InvalidDigest is returned.

## Example {#section_ovw_mcw_bz .section}

**Example of a request with no referer contained:**

```
PUT /? referer HTTP/1.1
Host: BucketName.oss.example.com
Content-Length: 247
Date: Fri, 04 May 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=

<? xml version="1.0" encoding="UTF-8"? >
<RefererConfiguration>
<AllowEmptyReferer>true</AllowEmptyReferer >
< RefererList />
</RefererConfiguration>

```

**Example of a request with referer contained:**

```
PUT /? referer HTTP/1.1
Host: BucketName.oss.example.com
Content-Length: 247
Date: Fri, 04 May 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=

<? xml version="1.0" encoding="UTF-8"? >
<RefererConfiguration>
<AllowEmptyReferer>true</AllowEmptyReferer >
< RefererList>
<Referer> http://www.aliyun.com</Referer>
<Referer> https://www.aliyun.com</Referer>
<Referer> http://www. *.com</Referer>
<Referer> https://www.?.aliyuncs.com</Referer>
</ RefererList>
</RefererConfiguration>

```

**Response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

