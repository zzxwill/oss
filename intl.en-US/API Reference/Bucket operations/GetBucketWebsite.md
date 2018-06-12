# GetBucketWebsite {#reference_wvy_s4w_tdb .reference}

GetBucketWebsite operation is used to view the static website hosting status of a bucket.

## Request syntax {#section_jbk_tgw_bz .section}

```
GET /? website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response {#section_aqn_ygw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|ErrorDocument|Container| The parent element of the child element keyParent element: WebsiteConfiguration

|
|IndexDocument|Container|The parent element of the child element suffix Parent element: WebsiteConfiguration

|
|Key|String|The file name used to return Error 404  Parent element: WebsiteConfiguration.ErrorDocumen

This element is required when ErrorDocument is set

|
|Suffix|String|The index file name added when a directory URL is returned. This element cannot be empty or contain a slash \(/\). For example, if the index file index.html is configured, oss-cn-hangzhou.aliyuncs.com/mybucket/mydir/ contained in an access request is converted into oss-cn-hangzhou.aliyuncs.com/mybucket/index.html by default.  Parent element: WebsiteConfiguration.IndexDocument

|
|WebsiteConfiguration|Container|Requested container  Parent element: none

|

## Detail analysis {#section_mkj_ghw_bz .section}

-   If a bucket does not exist, the error “404 no content” is returned. Error code: NoSuchBucket.
-   Only the owner of a bucket can view the static website hosting status of the bucket. If other users attempt to access the status information, the error 403 Forbidden with the error code: AccessDenied is returned.
-   If the source bucket is not configured with static website hosting, OSS returns Error 404 with the error code: NoSuchWebsiteConfiguration.

## Example {#section_wld_hhw_bz .section}

**Request example:**

```
    Get /? website HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com   
    Date: Thu, 13 Sep 2012 07:51:28 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=

```

**Response example with logging rules configured:**

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu, 13 Sep 2012 07:51:28 GMT
Connection: keep-alive
Content-Length: 218  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<Websiteconfiguration xmlns = "http://doc.oss-cn-hangzhou.aliyuncs.com">
<IndexDocument>
<Suffix>index.html</Suffix>
    </IndexDocument>
    <ErrorDocument>
        <Key>error.html</Key>
    </ErrorDocument>
</WebsiteConfiguration>
```

**Return example with logging rules not set:**

```
HTTP/1.1 404 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu, 13 Sep 2012 07:56:46 GMT
Connection: keep-alive
Content-Length: 308  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<Error xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Code>NoSuchWebsiteConfiguration</Code>
    <Message>The specified bucket does not have a website configuration.</Message>
    <Bucketname> OSS-example </bucketname>
    <RequestId>505191BEC4689A033D00236F</RequestId>
    <HostId>oss-example.oss-cn-hangzhou.aliyuncs.com</HostId>
</Error>
```

