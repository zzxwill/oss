# DeleteObject {#reference_iqc_mqv_wdb .reference}

Deletes an object. To perform the DeleteObject operation on an object, you must have the write permission on the object.

**Note:** If the type of the requested object is symbol link, the DeleteObject operation only deletes the symbol link but not the content that the link directs to.

## Request syntax {#section_dcz_1rv_wdb .section}

```
DELETE /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Examples {#section_prc_gsv_wdb .section}

Request example:

```
DELETE /AK.txt HTTP/1.1
Host: test.oss-cn-zhangjiakou.aliyuncs.com
Accept-Encoding: identity
User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
Accept: */*
Connection: keep-alive
date: Wed, 02 Jan 2019 13:28:38 GMT
authorization: OSS qn6qrrqxo2oawuk53otfjbyc:zUglwRPGkbByZxm1+y4eyu+NIUs=zV0vhg=
Content-Length: 0
```

Response example:

```
HTTP/1.1 204 No Content
Server: AliyunOSS
Date: Wed, 02 Jan 2019 13:28:38 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5C2CBC8653718B5511EF4535
x-oss-server-time: 134
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage objects/Delete objects.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Manage objects/Delete objects.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/Python/Manage objects/Delete objects.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Manage objects/Delete objects.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Manage objects/Delete objects.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage objects/Delete objects.md)
-   [iOS](../../../../../intl.en-US//Manage objects.md)
-   [Node.js](../../../../../intl.en-US//Manage objects.md)
-   [Browser.js](../../../../../intl.en-US/SDK Reference/Browser.js/Manage objects.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Manage objects.md)

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|Not Found|404|The bucket in which the requested object is stored does not exist.|
|No Content|204|The requested object does not exist.|

