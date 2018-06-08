# GetBucketLogging {#reference_mm3_zwv_tdb .reference}

GetBucketLogging is used to view the access log configurations of a bucket.

## Request syntax {#section_cjt_lgw_bz .section}

```
GET /? logging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response  {#section_dvp_mgw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|BucketLoggingStatus|Container|The container for storing access log status informationChild element: LoggingEnabled

Parent element: none

|
|LoggingEnabled|Container|The container for storing access log information. This element is required only when server access logging is enabled. Child element: TargetBucket, TargetPrefix

Parent element: BucketLoggingStatus

|
|TargetBucket|Character|The bucket for storing access logs. Child element: none

Parent element: BucketLoggingStatus.LoggingEnabled

|
|TargetPrefix|Character|The prefix of the names of saved access log files. Child element: none

Parent element: BucketLoggingStatus.LoggingEnabled

|

## Detail analysis {#section_o5y_4gw_bz .section}

-   If a bucket does not exist, the error “404 no content” is returned. Error code: NoSuchBucket.
-   Only the owner of a bucket can view the access logging configuration of the bucket. If other users attempt to access the configuration, the error 403 Forbidden with the error code: AccessDenied is returned.
-   If no logging rules are set for the source bucket, OSS still returns an XML message body with the element BucketLoggingStatus being null.

## Example {#section_sss_pgw_bz .section}

**Request example:**

```
Get /? logging HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 04 May 2012 05:31:04 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=
```

**Response example with logging rules configured:**

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

**Response example with no logging rules configured:**

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

