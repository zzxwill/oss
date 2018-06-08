# GetObject {#reference_ccf_rgd_5db .reference}

GetObject is used to obtain an object. You must have read permission before you use Get Object function.

## Request syntax {#section_pjt_dmw_bz .section}

```
GET /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
Range: bytes=ByteRange (Optional)
```

## Request parameters {#section_gsg_gmw_bz .section}

When sending a GET request, you can customize some headers in OSS response. If you send an anonymous user request, you can only customize \`content-type\` headers. Other headers require that you send GET requests with signatures. These headers include:

|Name|Type|Description|
|----|----|-----------|
|response-content-type|string|Specifies the content-type header in the request returned by OSS. If you send an anonymous user request, you can also specify the content-type header. Default value: None

|
|response-content-language|string|Specifies the content-language header in the request returned by OSS. Default value: None

|
|response-expires|string|Specifies the expires header in the request returned by OSS. Default value: None

|
|response-cache-control|string|Specifies the cache-control header in the request returned by OSS. Default value: None

|
|Response-content-Disposition|string|Specifies the content-disposition header in the request returned by OSS. Default value: None

|
|response-content-encoding|string|Specifies the content-encoding header in the request returned by OSS. Default value: None

|

## Request header {#section_ywk_qmw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|Range|string|Specifies the range of file transfer. For example, if the range is set to bytes = 0-9, the system transfers byte 0 to byte 9. Default value: None

|
|If-Modified-Since|string|If the specified time is earlier than the actual modification time, the system transfers the file normally, and returns the 200 OK message; otherwise, the system returns the 304 Not Modified message. Default value: None

Time format: GMT, for example: Fri, 13 Nov 2015 14:47:53 GMT

|
|If-Unmodified-Since|string|If the specified time is same as or later than the actual file modification time, the system transfers the file normally, and returns the 200 OK message; otherwise, the system returns the 412 Precondition Failed message. Default value: None

Time format: GMT, for example: Fri, 13 Nov 2015 14:47:53 GMT

|
|If-Match|string|If the expected ETag that is introduced matches the ETag of the object, the system transfers the file normally, and returns the 200 OK message; otherwise, the system returns the 412 Precondition Failed message. Default value: None

|
|If-None-Match|string|If the introduced ETag does not match the ETag of the object, the system transfers the file normally, and returns the 200 OK message; otherwise, the system returns the 304 Not Modified message. Default value: None

|

## Detail analysis {#section_xb4_wmw_bz .section}

-   The Range parameter in the Get Object request can be set to support resumable data transfer from breakpoints. This function is recommended if the object size is large.
-   If the Range parameter is used in the request header, the returned message includes the length of the entire file and the range returned for the request. For example, if the returned message is Content-Range: bytes 0-9/44, it means that the length of the entire file is 44, and the range returned is 0 to 9.  If the range requirement is not met, the system transfers the entire file and does not include Content-Range in the result.
-   If the time specified by If-Modified-Since does not match the actual modification time, the system directly returns the file and the 200 OK message.
-   If-Modified-Since can coexist with If-Unmodified-Since. If-Match can also coexist with If-None-Match.
-   If the request contains If-Unmodified-Since and If-Unmodified-Since does not match the actual modification time, or the request contains If-Match and If-Match does not match the Etag of the object, the system returns the 412 Precondition Failed message.
-   If the request contains If-Modified-Since and If-Modified-Since does not match the actual modification time, or the request contains If-None-Match and If-None-Match does not match the ETag of the object, the system returns Error 304 Not Modified.
-   If the file does not exist, the system returns Error 404 Not Found.  The error code is NoSuchKey.
-   OSS does not allow you to customize the headers in OSS returned request by using request parameters in the GET request during an anonymous access.
-   When you customize some headers in OSS returned request, OSS sets these headers to the values specified by parameters in the GET Object Request only when the request is successfully processed, that is, when the system returns the 200 OK message.
-   If this object is encrypted on the server side, the system automatically returns the decrypted object on receiving the GET Object request, and returns x-oss-server-side-encryption in the response header. The value of x-oss-server-side-encryption indicates the server-side encryption algorithm of the object.
-   If you want to compress and transfer the returned content using GZIP, add Accept-Encoding:gzip to the display mode in the request header. OSS determines whether to return the data compressed by GZIP to you based on the Content-Type and size of the file. If the content is compressed using GZIP, the content does not contain the Etag.  Currently, OSS supports GZIP compression for the following Content-Types: HTML, Javascript, CSS, XML, RSS, and JSON, and the file size must be at least 1 KB.
-   If the file type is symbolic link, the content of the target file is returned. In the response header, `Content-Length`, `ETag`, and `Content-Md5` are metadata of the target file, `Last-Modified` is the maximum value of the target file and symbolic link, and others are metadata of symbolic links.
-   If the file type is symbolic link and the target file does not exist, the system returns Error 404 Not Found. The error code is SymlinkTargetNotExist.
-   If the file type is symbolic link and the target file type is symbolic link, the system returns Error 400 Bad request. The error code is InvalidTargetType.
-   For the Archive type, submit the Restore request and complete Restore before downloading an object. The object can be downloaded only when the Restore operation is completed and not timed-out.
    -   If the Restore request is not submitted or the last Restore operation is time-out, the system returns Error 403. The error code is InvalidObjectState.
    -   If the Restore request has been submitted but the Restore operation is not completed, the system returns Error 403. The error code is InvalidObjectState.
    -   Data can be directly downloaded only when the Restore operation is completed and not timed-out.

## Example {#section_vrp_zmw_bz .section}

**Request example:**

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

**Request example with range specified:**

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

**Request example with the returned message header customized:**

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

**Request example of symbolic link:**

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

**Example of a request when the Restore operation of an object of the Archive type is completed:**

```
GET /oss.jpg HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 09:38:30 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:zUglwRPGkbByZxm1+y4eyu+NIUs=
```

**Response example**

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

