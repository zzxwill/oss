# GetObjectMeta {#reference_sg4_k2w_wdb .reference}

Obtains the metadata of an object in a bucket, including the ETag, Size, and LastModified. The content of the object is not returned.

**Note:** 

-   If the requested object is a symbol link, the information of the symbol link is returned.
-   The response to a GetObjectMeta request does not include a message body whether the request is successful.

## Request syntax {#section_nvn_42w_wdb .section}

```
HEAD /ObjectName?objectMeta HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response headers {#section_und_k4s_qgb .section}

|Header|Type|Description|
|:-----|----|:----------|
|Content-Length|String| Indicates the size of the object.

 |
|ETag|String| Indicates the ETag of the object, which is generated when an object is created to identify the content of the object.

 For an object created by a PutObject request, its ETag is the MD5 value of its content. For an object created in other methods, its ETag is the UUID of its content. The ETag of an object can be used to check whether the content of the object changes. We recommend you do not use ETag as the MD5 value of an object to verify data integrity.

 Default value: None

 |

## Examples {#section_wkh_52w_wdb .section}

Request example:

```
HEAD /oss.jpg?objectMeta HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Apr 2015 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0FmgbrQ0=
```

Response example:

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Apr 2015 05:21:12 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE"
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
Content-Length: 344606
Connection: keep-alive
Server: AliyunOSS
```

## SDK {#section_pfh_pnw_tgb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage objects/Manage Object Meta.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Manage objects/Manage Object Meta.md)
-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage objects/Manage Object Meta.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Manage objects/Manage Object Meta.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Manage objects/Manage Object Meta.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Manage objects/Manage ACL for an object.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Manage objects/Manage ACL for an object.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage objects/Manage Object Meta.md)
-   [iOS](../../../../../intl.en-US//Manage objects.md)

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|Not Found|404|The requested object does not exist.|

