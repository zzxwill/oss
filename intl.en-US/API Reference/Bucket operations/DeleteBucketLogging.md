# DeleteBucketLogging {#reference_jrn_gsw_tdb .reference}

Disables the access logging function of a bucket. Only the owner of a bucket can disable the access logging function of the bucket.

## Request syntax {#section_lyn_m3w_bz .section}

```
DELETE /? logging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Examples {#section_isc_43w_bz .section}

Request example:

```
DELETE /? logging HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:35:24 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:6ZVHOehYzxoC1yxRydPQs/CnMZU=

```

Response example:

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 05:35:24 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Set logging.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Set logging.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Set logging.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Set logging.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Set logging.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Set logging.md)
-   [Node.js](../../../../../intl.en-US/SDK Reference/Node. js/Set logging.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Set logging.md)

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The target bucket does not exist.|
|AccessDenied|403|You do not have the permission to disable the access logging function of the bucket. Only the owner of a bucket can disable the access logging function of the bucket.|

