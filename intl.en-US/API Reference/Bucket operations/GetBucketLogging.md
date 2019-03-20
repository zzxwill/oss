# GetBucketLogging {#reference_mm3_zwv_tdb .reference}

Views the access logging configuration of a bucket. Only the owner of a bucket can view the access logging configuration of the bucket.

## Request syntax {#section_cjt_lgw_bz .section}

```
GET /? logging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_dvp_mgw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|BucketLoggingStatus|Container|Indicates the container used to store access logging configuration of a bucket.Sub-node: LoggingEnabled

Parent node: None

**Note:** If no logging rules are set for the source bucket, OSS returns an XML message body in which the value of BucketLoggingStatus is null.

|
|LoggingEnabled|Container|Indicates the container used to store access logging information. This element is returned if it is enabled and is not returned if it is disabled.Sub-node: TargetBucket and TargetPrefix

Parent node: BucketLoggingStatus

|
|TargetBucket|Character|Indicates the bucket that stores access logs. Sub-node: None

Parent node: BucketLoggingStatus.LoggingEnabled

|
|TargetPrefix|Character|Indicates the prefix of the names of stored access log files. Sub-node: None

Parent node: BucketLoggingStatus.LoggingEnabled

|

## Examples {#section_sss_pgw_bz .section}

Request example:

```
Get /? logging HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 04 May 2012 05:31:04 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=
```

Response example returned when logging rules are set for the bucket:

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 05:31:04 GMT
Connection: keep-alive
Content-Length: 210  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<BucketLoggingStatus xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <LoggingEnabled>
        <TargetBucket>mybucketlogs</TargetBucket>
        <TargetPrefix>mybucket-access_log/</TargetPrefix>
    </LoggingEnabled>
</BucketLoggingStatus>
```

Response example returned when no logging rules are set for the bucket:

```
HTTP/1.1 200 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 05:31:04 GMT
Connection: keep-alive
Content-Length: 110  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<BucketLoggingStatus xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
</BucketLoggingStatus>
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
|AccessDenied|403|You do not have the permission to view the access logging configuration of a bucket. Only the owner of a bucket can view the access logging configuration of the bucket.|

