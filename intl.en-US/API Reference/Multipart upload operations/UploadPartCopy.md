# UploadPartCopy {#reference_t4b_vpx_wdb .reference}

UploadPartCopy uploads a part by copying data from an existing object.

You can add an x-oss-copy-source header in the Upload Part request to call the Upload Part Copy interface. When copying a file larger than 1 GB, you must use the Upload Part Copy method. For the Upload Part Copy operation,  the source bucket and the target bucket must be in the same region. If you want to copy a file that is less than 1 GB by a single operation, you can use the Copy Object method.

## Request syntax  {#section_gqh_crx_wdb .section}

```
PUT /ObjectName? partNumber=PartNumber&uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: Size
Authorization: SignatureValue
x-oss-copy-source: /SourceBucketName/SourceObjectName
x-oss-copy-source-range:bytes=first-last
```

## Request header {#section_d23_2rx_wdb .section}

Except the common request header, other headers in the Upload Part Copy request are used to specify the address of the copied source object and copying range.

|Name |Type |Description |
|:----|:----|:-----------|
|x-oss-copy-source|String|Specifies the copy source address \(the requester must have the permission to read the source object\).  Default: None

|
|x-oss-copy-source-range|Integer|Copying range of the copied source object. For example, if the range is set to bytes = 0-9, the system transfers byte 0 to byte 9.  This request header is not required when the entire source object is copied. Default: None

|

The following request header is used for the source objects specified by x-oss-copy-source.

|Name|Type|Description|
|:---|:---|:----------|
|x-oss-copy-source-if-match|String|If the ETag value of the source object is equal to the ETag value provided by the user, the system performs the Copy Object operation; otherwise, the system returns the 412 Precondition Failed error. Default: None

|
|x-oss-copy-source-if-none-match|String|If the source object has not been modified since the time specified by the user, the system performs the Copy Object operation; otherwise, the system returns the 412 Precondition Failed error.Default: None

|
|x-oss-copy-source-if-unmodified-since|String|If the time specified by the received parameter is the same as or later than the modification time of the file, the system transfers the file normally, and returns 200 OK; otherwise, the system returns the 412 Precondition Failed error. Default: None

|
|x-oss-copy-source-if-modified-since|String|If the source object has been modified since the time specified by the user, the system performs the Copy Object operation; otherwise, the system returns the 412 Precondition Failed error. Default: None

|

## Response elements {#section_q3q_srx_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|x-oss-copy-source-if-match|String|If the ETag value of the source object is equal to the ETag value provided by the user, the system performs the Copy Object operation; otherwise, the system returns the 412  Precondition Failed error. Default: None

|
|x-oss-copy-source-if-none-match|String|If the source object has not been modified since the time specified by the user, the system performs the Copy Object operation; otherwise, the system returns the 412  Precondition Failed error. Default: None

|
|x-oss-copy-source-if-unmodified-since|String|If the time specified by the received parameter is the same as or later than the modification time of the file, the system transfers the file normally, and returns 200 OK; otherwise, the system returns the 412 Precondition Failed error.Default: None

|
|x-oss-copy-source-if-modified-since|String|If the source object has been modified since the time specified by the user, the system performs the Copy Object operation; otherwise, the system returns the 412  Precondition Failed error.Default: None

|

## Detail analysis {#section_nbb_dsx_wdb .section}

-   Before calling the InitiateMultipartUpload interface to upload a part of data, you must call this interface to obtain an Upload ID issued by the OSS server.
-   In the Multipart Upload mode, besides the last part, all other parts must be larger than 100 KB. However, the Upload Part interface does not immediately verify the size of the uploaded part \(because it cannot immediately determine which part is the last one\). It verifies the size of the uploaded part only when Multipart Upload is completed.
-   If the x-oss-copy-source-range request header is not specified, the entire source object is copied. If the request header is specified, the returned message includes the length of the entire file and the COPY range. For example, if the returned message is Content-Range: bytes 0-9/44, which means that the length of the entire file is 44, and the COPY range is 0 to 9.  If the specified range does not conform to the range rules, OSS copies the entire file and does not contain Content-Range in the result.
-   If the x-oss-server-side-encryption request header is specified when the InitiateMultipartUpload interface is called,  OSS encrypts the uploaded part and return the x-oss-server-side-encryption header in the Upload Part response header. The value of x-oss-server-side-encryption indicates the server-side encryption algorithm used for this part. For more information, see the InitiateMultipartUpload API.
-   This operation cannot be used to copy objects created by Append Object.
-   If the bucket type is Archive, you cannot call this interface; otherwise, the system returns Error 400 with the error code “OperationNotSupported”.

## Example {#section_sv4_hsx_wdb .section}

**Request example: **

```
PUT /multipart.data? Partnumber = 1 & sealadid = porterhttp/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length：6291456
Date: Wed, 22 Feb 2012 08:32:21 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:J/lICfXEvPmmSW86bBAfMmUmWjI=
x-oss-copy-source: /oss-example/ src-object
x-oss-copy-source-range:bytes=100-6291756
```

**Response example:**

```
HTTP/1.1 200 OK 
Server: AliyunOSS
Connection: keep-alive
x-oss-request-id: 3e6aba62-1eae-d246-6118-8ff42cd0c21a
Date: Thu, 17 Jul 2014 06:27:54 GMT'
<? xml version="1.0" encoding="UTF-8"? >
<CopyPartResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <LastModified>2014-07-17T06:27:54.000Z </LastModified>
    <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE"</ETag>
</CopyPartResult>
```

