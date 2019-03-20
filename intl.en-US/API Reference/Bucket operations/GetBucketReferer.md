# GetBucketReferer {#reference_bs5_rpw_tdb .reference}

Views the referer configuration of a bucket. Only the owner of a bucket can view the referer configuration of the bucket.

## Request syntax {#section_j24_4hw_bz .section}

```
GET /? referer HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_jzk_phw_bz .section}

|Element|Type|Description|
|-------|----|-----------|
|RefererConfiguration|Container|Indicates the container that stores the referer configuration of the bucket. Sub-node: AllowEmptyReferer and RefererList

Parent node: None

|
|AllowEmptyReferer|Enumerated string|Specifies whether the access request in which the referer field is null is allowed.  Valid value: true or false

Default value: true

Parent node: RefererConfiguration

|
|RefererList|Container |Indicates the container that stores the referer access whitelist for the bucket.Sub-node: Referer

Parent node: RefererConfiguration 

|
|Referer|String|Specifies a referer access whitelist.  Parent node: RefererList

|

## Detail analysis {#section_gvp_rhw_bz .section}

-   If the bucket does not exist, error 404 is returned. Error code: NoSuchBucket.
-   Only the owner of a bucket can view the referer configuration of the bucket. If other users attempt to access the configuration, the error 403 Forbidden with the error code: AccessDenied is returned.
-   If no referer configuration has been conducted for the bucket, OSS returns the default AllowEmptyReferer value and an empty RefererList.

## Examples {#section_hr3_shw_bz .section}

Request example:

```
Get /? referer HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Thu, 13 Sep 2012 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=
```

Response example returned when a referer rule is configured for the bucket:

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

Response example returned when no referer rule is configured for the bucket:

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
|NoSuchBucket|404|The target bucket does not exist.|
|AccessDenied|403|You do not have the permission to view the referer configuration of a bucket. Only the owner of a bucket can view the referer configuration of the bucket.|

