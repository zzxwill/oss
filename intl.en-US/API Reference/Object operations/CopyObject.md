# CopyObject {#reference_mvx_xxc_5db .reference}

CopyObject is used to copy an existing object in OSS into another object.

You can send a PUT request to OSS, and add the element “x-oss-copy-source” to the PUT request header to specify the copy source. OSS automatically determines that this is a Copy Object operation, and directly performs this operation on the server side. If the Copy Object operation is successful, the system returns new object information.

This operation is applicable to a file smaller than 1 GB. To copy a file greater than 1 GB, you must use the Multipart Upload operation. For more information about this operation, see [UploadPartCopy](intl.en-US/API Reference/Multipart upload operations/UploadPartCopy.md#).

**Note:** For the Copy Object operation, the source bucket and the target bucket must be in the same region.

## Request syntax {#section_jzf_fmr_xdb .section}

```
PUT /DestObjectName HTTP/1.1
Host: DestBucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-copy-source: /SourceBucketName/SourceObjectName
```

## Request header {#section_vdd_klw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|x-oss-copy-source|String|Specifies the copy source address \(the requester must have the permission to read the source object\). Default: None

|
|x-oss-copy-source-if-match|String|If the source object’s ETag value is same as the ETag value provided by the user, a COPY operation is executed, and the code 200 is returned. Otherwise, the system returns the HTTP error code 412 \(preprocessing failed\).  Default: None

|
|x-oss-copy-source-if-none-match|String|If the source object’s ETag value is not the same as the ETag value provided by the user, a COPY operation is executed, and the code 200 is returned. Otherwise, the system returns the HTTP error code 304 \(preprocessing failed\).  Default: None

|
|x-oss-copy-source-if-unmodified-since|String|If the time specified by the received parameter is same as or later than the modification time of the file, the system transfers the file normally, and returns 200 OK; otherwise, the system returns 412 Precondition Failed.  Default: None

|
|x-oss-copy-source-if-modified-since|String|If the source object has been modified after the time specified by the user, the system performs a COPY operation. Otherwise, the system returns the 304 HTTP error code \(preprocessing failed\).  Default: None

|
|x-oss-metadata-directive|String|Valid values include COPY and REPLACE. If this parameter is set to COPY, the system copies meta for the new object from the source object. If this parameter is set to REPLACE, the system ignores all meta values of the source object, and uses the meta value specified in this request. If this parameter is set to a value other than COPY and REPLACE, the system returns the 400 Bad Request message. Note that when the value is COPY, the source object’s x-oss-server-side-encryption meta value cannot be copied.Default value: COPY

Valid values:COPY and REPLACE

|
|x-oss-server-side-encryption|String|Specifies the server-side entropy encryption algorithm when OSS creates the target object. Valid values: AES256 or KMS

**Note:** You must enable the KMS \(Key Management Service\) on the console to use the KMS encryption algorithm. Otherwise, a KmsServiceNotenabled error code is reported.

|
|x-oss-object-acl|String|Specifies the access permission when OSS creates an object. Valid values: public-read, private, public-read-write

|

## Response elements {#section_tvz_qlw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|CopyObjectResult|String|Object copying resultDefault: None

|
|ETag|String|ETag value of the new object.Parent element: CopyObjectResult

|
|LastModified|String|Last update time of the new object.Parent element: CopyObjectResult

|

## Detail analysis {#section_cqk_tlw_bz .section}

-   You can use the Copy Object operation to modify the meta information of an existing object.
-   If the source object address is the same as the target object address in the Copy Object operation, the system directly replaces the meta information in the source object regardless of the value of x-oss-metadata-directive.
-   OSS allows the Copy Object request to contain any number of the four pre-judgment headers. For more information about the related logic, see Detail Analysis of Get Object.
-   To complete a Copy Object operation, the requester must have the permission to read the source object.
-   The source object and the target object must belong to the same data center. Otherwise, the system returns the error code 403 AccessDenied. The error message is Target object does not reside in the same data center as source object.
-   In the billing statistics of the Copy Object operation, the number of Get requests increases by 1 in the bucket of the source object, the number of Put requests increases by 1 in the bucket of the target object, and a storage space is added accordingly.
-   In the Copy Object operation, all relevant request headers start from x-oss-, and therefore must be added to the signature string.
-   If the x-oss-server-side-encryption header is specified in the Copy Object request, and its value \(AES256\) is valid, the target object is encrypted on the server side after the Copy Object operation is performed no matter whether the source object has been encrypted on the server side. In addition, the Copy Object response header contains x-oss-server-side-encryption, the value of which is set to the encryption algorithm of the target object. When this target object is downloaded, the response header also contains x-oss-server-side-encryption, the value of which is set to the encryption algorithm of this target object. If the x-oss-server-side-encryption request header is not specified in the Copy Object operation, the target object is the data that is not encrypted on the server side even if the source object has been encrypted on the server side.
-   When the x-oss-metadata-directive header in the Copy Object request is set to COPY \(default value\), the system does not copy the x-oss-server-side-encryption value of the source object. That is, the target object is encrypted on the server side only when x-oss-server-side-encryption is specified accordingly in the Copy Object request.
-   When the x-oss-server-side-encryption request header is specified in the COPY operation, and the request value is not AES256, the system returns Error 400 with the error code “InvalidEncryptionAlgorithmError”.
-   If the size of the file to be copied is greater than 1 GB, the system returns Error 400 with the error code “EntityTooLarge”.
-   This operation cannot be used to copy objects created by Append Object.
-   If the file type is symbolic link, copy the symbolic link only.

## Example {#section_osk_5lw_bz .section}

**Request example:**

```
PUT /copy_oss.jpg HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 07:18:48 GMT
x-oss-copy-source: /oss-example/oss.jpg
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:gmnwPKuu20LQEjd+iPkL259A+n0=
```

**Return example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Content-Type: application/xml
Content-Length: 193
Connection: keep-alive
Date: Fri, 24 Feb 2012 07:18:48 GMT
Server: AliyunOSS
<? xml version="1.0" encoding="UTF-8"? >
<CopyObjectResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
 <LastModified>Fri, 24 Feb 2012 07:18:48 GMT</LastModified>
 <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE"</ETag>
</CopyObjectResult>
```

