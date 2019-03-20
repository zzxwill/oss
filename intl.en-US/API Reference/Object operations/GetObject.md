# GetObject {#reference_ccf_rgd_5db .reference}

Obtains an object. To perform GetObject operations, you must have the read permission on the object.

**Note:** If the storage class of the request object is Archive, you must send a RestoreObject request first and ensure that the request is successfully responded without timeout.

## Request syntax {#section_pjt_dmw_bz .section}

```
GET /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
Range: bytes=ByteRange (optional)
```

## Request parameters {#section_gsg_gmw_bz .section}

**Note:** 

-   You can customize some headers in the response to a GET request by setting headers in the GET request. However, the headers in the response are set to the values specified in the GET request headers only when the request is successful \(the 200 OK code is returned\).
-   You cannot customize response headers by setting headers in the GET request as an anonymous user.
-   You must sign the GET request before sending it.

The following table describes the headers that you can customize:

|Header|Type|Required|Description|
|------|----|--------|-----------|
|response-content-type|String|No|Specifies the content-type header in the request returned by OSS. Default value: None

|
|response-content-language|String|No|Specifies the content-language header in the response returned by OSS. Default value: None

|
|response-expires|String|No|Specifies the expires header in the response returned by OSS. Default value: None

|
|response-cache-control|String|No|Specifies the cache-control header in the response returned by OSS. Default value: None

|
|response-content-disposition|String|No|Specifies the content-disposition header in the response returned by OSS. Default value: None

|
|response-content-encoding|String|No|Specifies the content-encoding header in the response returned by OSS. Default value: None

|
|Range|String|No| Specifies the range of object that is transmitted.

 Default value: None

 -   If the value of Range is valid, the total size of the object and the range of the returned object are included in the response. For example, "Content-Range: bytes 0-9/44" indicates that the total size of the object is 44, and the data in the range of 0-9 is returned.
-   If the value of Range is invalid, the entire object is transmitted, and Content-Range is not included in the response.

 |
|If-Modified-Since|String|No|If the time specified in the parameter is earlier than the modification time or does not conform to the standards, OSS returns the object and the 200 OK message. Otherwise, the 304 Not Modified message is returned.Default value: None

Time format: GMT, for example: Fri, 13 Nov 2015 14:47:53 GMT.

|
|If-Unmodified-Since|String|No|If the time specified in the parameter is the same as or later than the modification time of the object, OSS returns the object and the 200 OK message. Otherwise, the 412 Precondition Failed error message is returned.Default value: None

Time format: GMT, for example: Fri, 13 Nov 2015 14:47:53 GMT.

You can specify the If-Modified-Since and If-Unmodified-Since parameters in a request at the same time.

|
|If-Match|String|No|If the expected ETag that is introduced matches the ETag of the object, OSS transmits the object normally and returns the 200 OK message. Otherwise, the 412 Precondition Failed message is returned.Default value: None

|
|If-None-Match|String|No|If the introduced ETag does not match the ETag of the object, OSS transmits the object normally and returns the 200 OK message. Otherwise, the system returns the 304 Not Modified message.Default value: None

You can specify the If-Match and If-None-Match parameters in a request at the same time.

|
|Accept-Encoding|String|No| Specifies the encoding type at the client-side.

 If you want an object to be returned in the GZIP format, explicitly add Accept-Encoding:gzip in the request header. OSS determines whether to return the object compressed in the GZIP format based on the Content-Type and size of the object \(larger than or equal to 1 KB\).

 **Note:** 

-   If an object is compressed in the GZIP format, the ETag of the object is not included in the returned result.
-   Currently, OSS supports GZIP compression for the following Content-Types: HTML, Javascript, CSS, XML, RSS, and JSON.

 |

## Response header {#section_ump_3hn_xgb .section}

**Note:** If the type of the requested object is symbol link, the content of the object is returned. In the response header, `Content-Length`, `ETag`, and `Content-Md5` are the metadata of the requested object, `Last-Modified` is the maximum value of the requested object and symbol link \(that is, the later modification time\), and other parameters are the metadata of the symbol link.

|Header|Type|Description|
|------|----|-----------|
|x-oss-server-side-encryption|String|If the requested object is encrypted with the entropy coding algorithm on the server, OSS decrypts the object and includes this header in the response to indicates the encryption algorithm used to encrypt the object on the server.|

## Examples {#section_vrp_zmw_bz .section}

**Simple request example:**

```
GET /oss.jpg HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 06:38:30 GMT
Authorization:OSS qn6qrrqxo2oawuk53otfjbyc:UNQDb7GapEgJCZkcde6OhZ9Jfe8=
```

**Response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 3a89276f-2e2d-7965-3ff9-51c875b99c41
x-oss-object-type: Normal
Date: Fri, 24 Feb 2012 06:38:30 GMT
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE "
Content-Type: image/jpg
Content-Length: 344606
Server: AliyunOSS
[344606 bytes of object data]
```

**Request example with Range specified:**

```
GET //oss.jpg HTTP/1.1
Host:oss-example. oss-cn-hangzhou.aliyuncs.com
Date: Fri, 28 Feb 2012 05:38:42 GMT
Range: bytes=100-900
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:qZzjF3DUtd+yK16BdhGtFcCVknM=
```

**Response example:**

```
HTTP/1.1 206 Partial Content
x-oss-request-id: 28f6508f-15ea-8224-234e-c0ce40734b89
x-oss-object-type: Normal
Date: Fri, 28 Feb 2012 05:38:42 GMT
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE "
Accept-Ranges: bytes
Content-Range: bytes 100-900/344606
Content-Type: image/jpg
Content-Length: 801
Server: AliyunOSS
[801 bytes of object data]
```

**Request example with returned message headers customized:**

```
GET /oss.jpg? response-expires=Thu%2C%2001%20Feb%202012%2017%3A00%3A00%20GMT& response-content-type=text&response-cache-control=No-cache&response-content-disposition=attachment%253B%2520filename%253Dtesting.txt&response-content-encoding=utf-8&response-content-language=%E4%B8%AD%E6%96%87 HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com:
Date: Fri, 24 Feb 2012 06:09:48 GMT
```

**Response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
x-oss-object-type: Normal
Date: Fri, 24 Feb 2012 06:09:48 GMT 
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE "
Content-Length: 344606
Connection: keep-alive
Content-disposition: attachment; filename:testing.txt
Content-language: Chinese
Content-encoding: utf-8
Content-type: text
Cache-control: no-cache
Expires: Fri, 24 Feb 2012 17:00:00 GMT
Server: AliyunOSS
[344606 bytes of object data]
```

**Request example with the object type specified as symbol link:**

```
GET /link-to-oss.jpg HTTP/1.1
Accept-Encoding: identity
Date: Tue, 08 Nov 2016 03:17:58 GMT
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Authorization:OSS qn6qrrqxo2oawuk53otfjbyc:qZzjF3DUtd+yK16BdhGtFcCVknM=
```

**Response example:**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Tue, 08 Nov 2016 03:17:58 GMT
Content-Type: application/octet-stream
Content-Length: 20
Connection: keep-alive
x-oss-request-id: 582143E6D3436A212ADCC87D
Accept-Ranges: bytes
ETag: "8086265EFC0211ED1F9A2F09BF462227"
Last-Modified: Tue, 08 Nov 2016 03:17:58 GMT
x-oss-object-type: Symlink
Content-MD5: gIYmXvwCEe0fmi8Jv0YiJw==
```

**Request example for an Archive object that is restored:**

```
GET /oss.jpg HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 09:38:30 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:zUglwRPGkbByZxm1+y4eyu+NIUs=
```

**Response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 58F723894529F18D7F000053
x-oss-object-type: Normal
x-oss-restore: ongoing-request="false", expiry-date="Sun, 16 Apr 2017 08:12:33 GMT"
Date: Sat, 15 Apr 2017 09:38:30 GMT
Last-Modified: Sat, 15 Apr 2017 06:07:48 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE "
Content-Type: image/jpg
Content-Length: 344606
Server: AliyunOSS
[354606 bytes of object data]
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Upload objects/Overview.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Upload objects/Overview.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Upload objects/Overview .md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Upload objects/Overview.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Upload objects/Overview.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Upload objects/Overview.md)
-   [Node.js](../../../../../intl.en-US//Upload objects.md)
-   [Browser.js](../../../../../intl.en-US/SDK Reference/Browser.js/Upload objects.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Upload objects.md)

## Error codes {#section_hnc_tz5_jgb .section}

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|NoSuchKey|404|The requested object does not exist.|
|SymlinkTargetNotExist|404|The requested object is a symbol link and does not exist.|
|InvalidTargetType|400|The requested object is a symbol link.|
|InvalidObjectState|403|The storage class of the requested object is Archive and: -   The RestoreObject request for the object is not initiated or timed out.
-   The RestoreObject request for the object has been initiated but the object is not restored yet.

|
|Not Modified|304| -   The If-Modified-Since header is specified in the request, but the source object has not been modified after the time specified in the request.
-   The If-None-Match header is specified in the request, and the ETag provided in the request is the same as the ETag of the source object.

 |
|Precondition Failed|412| -   The If-Unmodified-Since header is specified, but the time specified in the request is earlier than the modification time of the object.
-   The If-Match header is specified, but the provided ETag is different from the ETag of the source object.

 |

