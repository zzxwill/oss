# PutBucketReferer {#reference_prc_ys5_tdb .reference}

Sets the referer access whitelist of a bucket and configures whether a request in which the referer field is null is allowed. 

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

|Element|Type|Required|Description|
|-------|----|--------|-----------|
|RefererConfiguration|Container|Yes|Specifies the container that stores the referer settings.Sub-nodes: AllowEmptyReferer and RefererList 

Parent node: None

|
|AllowEmptyReferer|Enumerated string |Yes|Specifies whether a request in which the referer field is null is allowed. The specified value replaces the previous AllowEmptyReferer setting .Valid value: true or false

Default value:true

Parent node: RefererConfiguration 

 |
|RefererList|Container|Yes|Specifies the container that stores the referer access whitelist.**Note:** The PutBucketReferer operation replaces the configured whitelist with the whitelist specified in RefererList. If the value of ReferList is null \(that is, Referer is not included\) in the request, this operation replaces the configured whitelist with a null value, that is, deletes the configured RefererList.

Parent node: RefererConfiguration 

Sub-node: Referer

 |
|Referer|String|No|Specifies a referer access whitelist. Parent node: RefererList

|

## Detail analysis {#section_fcy_lcw_bz .section}

-   Only the bucket owner can initiate a Put Bucket Referer request. Otherwise, the message of 403 Forbidden is returned.  Error code: AccessDenied.
-   The configuration specified in AllowEmptyReferer replaces the previous AllowEmptyReferer configuration. This field is required. By default, AllowEmptyReferer in the system is configured as true.
-   This operation overwrites the previously configured whitelist with the whitelist in the RefererList. When the user-uploaded RefererList is empty \(containing no referer request element\), this operation overwrites the configured whitelist, that is, the previously configured RefererList is deleted.
-   If you have uploaded the Content-MD5 request header, OSS calculates the body’s Content-MD5 and checks if the two are the same. If the two are different, the error code: InvalidDigest is returned.

## Examples {#section_ovw_mcw_bz .section}

Example of a request with no referer contained:

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

Example of a request with referer contained:

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

Response example:

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Anti-leech.md)
-   [Python](../../../../../intl.en-US/SDK Reference/PHP/Anti-leech.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Anti-leech.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Anti-leech.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Anti-leech.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Anti-leech.md)
-   [Node.js](../../../../../intl.en-US/SDK Reference/Node. js/Anti-leech.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Anti-leech.md)

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|AccessDenied|403|You do not have the permission to perform this operation. Only the bucket owner can initiate a PutBucketReferer request.|
|InvalidDigest|400|If you include the Content-MD5 header in the request, OSS calculates the Content-MD5 of the request body and checks if the two are the same. If the two values are different, this error is returned.|

