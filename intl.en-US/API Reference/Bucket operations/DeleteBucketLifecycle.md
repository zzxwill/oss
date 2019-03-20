# DeleteBucketLifecycle {#reference_wl1_xsw_tdb .reference}

Deletes the lifecycle rules for a specified bucket. After you delete all lifecycle rules for a specified bucket by using this API, the objects stored in the bucket are no longer automatically deleted because of the lifecycle rules. Only the owner of a bucket can delete the lifecycle rules for the bucket.

## Request syntax {#section_krp_cjw_bz .section}

```
DELETE /?lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Examples {#section_qwg_2jw_bz .section}

Request example:

```
DELETE /?lifecycle HTTP/1.1
Host: BucketName.oss.aliyuncs.com  
Date: Mon, 14 Apr 2014 01:17:35 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:6ZVHOehYzxoC1yxRydPQs/CnMZU=

```

Response example:

```
HTTP/1.1 204 No Content 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Mon, 14 Apr 2014 01:17:35 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS

```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage lifecycle rules.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Manage lifecycle rules.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Manage lifecycle rules.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Manage lifecycle rules.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Manage lifecycle rules.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage lifecycle rules.md)
-   [Node.js](../../../../../intl.en-US/SDK Reference/Node. js/Manage lifecycle rules.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Manage lifecycle rules.md)

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The target bucket does not exist.|
|AccessDenied|403 Forbidden|You do not have the permission to delete the lifecycle rules for the bucket. Only the owner of a bucket can delete the lifecycle rules for the bucket.|

