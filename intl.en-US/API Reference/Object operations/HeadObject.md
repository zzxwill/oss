# HeadObject {#reference_bgh_cbw_wdb .reference}

HeadObject is used to return the meta information of a certain object without returning the file content.

## Request syntax {#section_wmz_jbw_wdb .section}

```
HEAD /ObjectName HTTP/1.1
Host: BucketName/oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request header {#section_xjh_mbw_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|If-Modified-Since|String|If the specified time is earlier than the actual modification time, the system returns the 200 OK message and the object metadata; otherwise, the system returns the 304 Not Modified message.  Default: None

 |
|If-Unmodified-Since|String|If the specified time is same as or later than the actual file modification time, the system returns the 200 OK message and the object metadata; otherwise, the system returns the 412 Precondition Failed message. Default: None

 |
|If-Match|String|If the expected ETag that is introduced matches the ETag of the object, the system returns the 200 OK message and the object metadata; otherwise, the system returns the 412 Precondition Failed message. Default: None

|
|If-None-Match|String|If the introduced ETag does not match the ETag of the object, the system returns the 200 OK message and the object metadata; otherwise, the system returns the 304 Not Modified message. Default: None

 |

## Detail analysis {#section_bxx_3cw_wdb .section}

-   After the Head Object request is sent, no message body is returned even if the system returns the 200 OK message or an error message.
-   The If-Modified-Since, If-Unmodified-Since, If-Match, and If-None-Match query conditions can be set in the header of the Head Object request. For the detailed setting rules, see the related fields in the Get Object request. If no modification is made, the system returns the 304 Not Modified message.
-   If you upload the user meta prefixed with x-oss-meta- when sending a Put Object request, for example, x-oss-meta-location, the user meta is returned.
-   If the file does not exist, the system returns Error 404 Not Found.
-   If this object is entropy encrypted on the server, the system returns x-oss-server-side-encryption in the header of the response to the Head Object request. The value of x-oss-server-side-encryption indicates the server-side encryption algorithm of the object.
-   If the file type is symbolic link, in the response header, `Content-Length`, `ETag`, and `Content-Md5` are metadata of the target file, `Last-Modified` is the maximum value of the target file and symbolic link, and others are metadata of symbolic links.
-   If the file type is symbolic link and the target file does not exist, the system returns Error 404 Not Found. The error code is “SymlinkTargetNotExist”.
-   If the file type is symbolic link and the target file type is symbolic link, the system returns Error 400 Bad request. The error code is “InvalidTargetType”.
-   If the bucket type is Archive and the Restore request has been submitted, the Restore state of Object is indicated by x-oss-restore in the response header.
    -   If the Restore request is not submitted or times out, the field is not returned.
    -   If the Restore request has been submitted and does not time out, the value of x-oss-restore returned is ongoing-request=”true”.
    -   If the Restore request has been submitted and completed, the value of x-oss-restore returned is ongoing-request=”false”, expiry-date=”Sun, 16 Apr 2017 08:12:33 GMT”. Where the expiry-date refers to the expiry date of the readable state of the restored file.

## Example {#section_fml_4dw_wdb .section}

**Request example:**

```
HEAD /oss.jpg HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 07:32:52 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:JbzF2LxZUtanlJ5dLA092wpDC/E=
```

**Return example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
x-oss-object-type: Normal
x-oss-storage-class: Archive
Date: Fri, 24 Feb 2012 07:32:52 GMT
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
ETag: "fba9dede5f27731c9771645a39863328"
Content-Length: 344606
Content-Type: image/jpg
Connection: keep-alive
Server: AliyunOSS
```

**Example of a request when the Restore request has been submitted but not completed:**

```
HEAD /oss.jpg HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 07:32:52 GMT
Authorization: OSS e1Unnbm1rgdnpI:KKxkdNrUBu2t1kqlDh0MLbDb99I=
```

**Return example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 58F71A164529F18D7F000045
x-oss-object-type: Normal
x-oss-storage-class: Archive
x-oss-restore: ongoing-request="true"
Date: Sat, 15 Apr 2017 07:32:52 GMT
Last-Modified: Sat, 15 Apr 2017 06:07:48 GMT
ETag: "fba9dede5f27731c9771645a39863328"
Content-Length: 344606
Content-Type: image/jpg
Connection: keep-alive
Server: AliyunOSS
```

**Example of a request when the Restore request has been submitted and completed:**

```
HEAD /oss.jpg HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 09:35:51 GMT
Authorization: OSS e1Unnbm1rgdnpI:21qtGJ+ykDVmdu6O6FMJnn+WuBw=
```

**Return example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 58F725344529F18D7F000055
x-oss-object-type: Normal
x-oss-storage-class: Archive
x-oss-restore: ongoing-request="false", expiry-date="Sun, 16 Apr 2017 08:12:33 GMT"
Date: Sat, 15 Apr 2017 09:35:51 GMT
Last-Modified: Sat, 15 Apr 2017 06:07:48 GMT
ETag: "fba9dede5f27731c9771645a39863328"
Content-Length: 344606
```

