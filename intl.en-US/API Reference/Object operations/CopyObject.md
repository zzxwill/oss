# CopyObject {#reference_mvx_xxc_5db .reference}

Copies objects within a bucket or between buckets in the same region. By calling CopyObject, you can send a PUT request to OSS. OSS automatically recognizes the request as a copy operation and perform it on the server.

## Limits {#section_pm3_8x3_4gp .section}

-   CopyObject only supports objects smaller than 1 GB. To copy objects larger than 1 GB, you must use [UploadPartCopy](reseller.en-US/API Reference/Multipart upload operations/UploadPartCopy.md#).
-   You can call CopyObject to modify the metadata of an object that equals to or smaller than 48.8 TB \(by setting the source object and target object to the same object\).
-   To use CopyObject, you must have the read permission on the source object.
-   The source object and the target object must be in the same region.
-   You cannot copy objects created by AppendObject.
-   If the source object is a symbolic link, only the symbolic link \(instead of the content that the link directs to\) is copied.

## Billing items {#section_fft_5uk_rc0 .section}

-   A GET request is billed according to the bucket where the source object is stored.
-   A PUT request is billed according to the bucket where the target object is stored.
-   The used storage capacity is billed according to the bucket where the target object is stored.
-   If you change the storage class of an object by calling CopyObject, the object is considered as overwritten and will incur charges. An object of the IA or Archive storage class will be charged if it is overwritten within 30 and 60 days respectively after it is created. For example, if you change the storage class of an object from IA to Archive or Standard 10 days after the object is created, early deletion fees for 20 days will be charged.

## Request syntax {#section_ts4_26v_wcl .section}

```
PUT /DestObjectName HTTP/1.1
Host: DestBucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-copy-source: /SourceBucketName/SourceObjectName
```

## Request header {#section_unr_cpz_y15 .section}

**Note:** The request headers used in copy operations start with x-oss-. Therefore, these headers must be added into the signature string.

|Header|Type|Required|Description|
|------|----|--------|-----------|
|x-oss-copy-source|String|Yes|Specifies the address of the source object. Default value: None.

 |
|x-oss-copy-source-if-match|String|No|If the ETag of the source object is the same as the ETag provided by the user, the copy operation is performed and a 200 OK message is returned. Otherwise, a 412 Precondition Failed error code \(preprocessing failed\) is returned. Default value: None.

 |
|x-oss-copy-source-if-none-match|String|No|If the ETag of the source object is different from the ETag provided by the user, the copy operation is performed and a 200 OK message is returned. Otherwise, a 304 Not Modified error code \(preprocessing failed\) is returned. Default value: None.

 |
|x-oss-copy-source-if-unmodified-since|String|No|If the specified time is the same as or later than the modification time of the object, the object is copied normally and a 200 OK message is returned. Otherwise, a 412 Precondition Failed error code \(preprocessing failed\) is returned. Default value: None.

 |
|x-oss-copy-source-if-modified-since|String|No|If the source object is modified after the time specified by the user, the copy operation is performed. Otherwise, a 304 Not Modified error code \(preprocessing failed\) is returned. Default value: None.

 |
|x-oss-metadata-directive|String|No| Specifies how to set the metadata of the target object. The valid values are COPY and REPLACE.

-   COPY \(default\): The metadata of the source object is copied to the target object. The x-oss-server-side-encryption of the source object is not copied. That is, server-side encryption is performed on the target object only if the x-oss-server-side-encryption header is specified in the COPY request.
-   REPLACE: The metadata of the target object is set to the metadata specified in the user's request instead of the metadata of the source object.

**Note:** If the source object and the target object have the same address, the metadata of the target object is replaced with the metadata of the source object regardless of the value of x-oss-metadata-directive.

 |
|x-oss-server-side-encryption|String|No|Specifies the server-side entropy encoding encryption algorithm when OSS creates the target object. Valid values:

-   AES256
-   KMS \(You must enable KMS in the console before you can use the KMS encryption algorithm. Otherwise, a KmsServiceNotEnabled error code is returned.\)

 **Note:** 

-   If the x-oss-server-side-encryption header is not specified in the copy operation, the target object is not encrypted on the server side no matter whether server-side encryption has been performed on the source object.
-   If you specify the x-oss-server-side-encryption header, server-side encryption is performed on the target object no matter whether the encryption has been performed on the source object. In addition, the response header for the copy request includes the x-oss-server-side-encryption header, and the value of the header is the encryption algorithm of the target object. When the target object is downloaded, the response header also includes the x-oss-server-side-encryption header, and the value of the header is the encryption algorithm of the target object.

 |
|x-oss-server-side-encryption-key-id|String|No|Indicates the primary key managed by KMS. This parameter is valid when the value of x-oss-server-side-encryption is KMS.

 |
|x-oss-object-acl|String|No|Specifies the ACL for the target object when it is created. Valid values:

-   public-read
-   private
-   public-read-write
-   default

 |
|x-oss-storage-class|String|No|Specifies the storage class of the object. Valid values:

-   Standard
-   IA
-   Archive

 Supported interfaces: PutObject, InitMultipartUpload, AppendObject, PutObjectSymlink, and CopyObject

 **Note:** 

-   If the value of StorageClass is invalid, a 400 error message is returned with an error code: InvalidArgument.
-   We recommend that you do not set the storage class to IA or Archive when calling CopyObject because an IA or Archive object smaller than 64 KB is billed at 64 KB.
-   If you specify the value of x-oss-storage-class when uploading an object to a bucket, the storage class of the uploaded object is the specified value of x-oss-storage-class. For example, if you specify the value of x-oss-storage-class to Standard when uploading an object to a bucket of the IA storage class, the storage class of the object is Standard.
-   If you change the storage class of an object, the object is considered as overwritten and will incur charges. An object of the IA or Archive class will be charged if it is overwritten within 30 and 60 days respectively after it is created.

 |
|x‑oss‑tagging|String|No| Specifies the tag of the object. You can set multiple tags at the same time, for example, TagA=A&TagB=B.

**Note:** You must perform URL encoding for the tag key and value in advance. If a tag does not contain an equal sign \(=\), this string does not have a value.

 |
|x‑oss-tagging-directive|String|No|Specifies how to set the tag of the target object. The valid values are Copy and Replace. -   Copy \(default\): The tag of the source object is copied to the target object.
-   Replace: The tag of the target object is set to the tag specified in the request instead of the tag of the source object.

 |

## Response elements {#section_nzu_6ah_dnw .section}

|Name|Type|Description|
|----|----|-----------|
|CopyObjectResult|String|Indicates the result of CopyObject. Default value: None.

 |
|ETag|String|Indicates the ETag of the target object. Parent node: CopyObjectResult

 |
|LastModified|String|Indicates the time when the target object is last modified. Parent node: CopyObjectResult

 |

## Examples {#section_wy8_69b_q8c .section}

-   Example 1

    Request example:

    ```
    PUT /copy_oss.jpg HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Fri, 24 Feb 2012 07:18:48 GMT 
    x-oss-storage-class: Archive
    x-oss-copy-source: /oss-example/oss.jpg 
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:gmnwPKuu20LQEjd+iPkL259A+n0=
    ```

    Response example:

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

-   Example 2

    Request example:

    ```
    PUT /test%2FAK.txt HTTP/1.1
    Host: tesx.oss-cn-zhangjiakou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: */*
    Connection: keep-alive
    x-oss-copy-source: /test/AK.txt
    date: Fri, 28 Dec 2018 09:41:55 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:gmnwPKuu20LQEjd+iPkL259A+n0=
    Content-Length: 0
    ```

    Response example:

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Fri, 28 Dec 2018 09:41:56 GMT
    Content-Type: application/xml
    Content-Length: 184
    Connection: keep-alive
    x-oss-request-id: 5C25EFE4462CE00EC6D87156
    ETag: "F2064A169EE92E9775EE5324D0B1682E"
    x-oss-hash-crc64ecma: 12753002859196105360
    x-oss-server-time: 150
    <? xml version="1.0" encoding="UTF-8"? >
    <CopyObjectResult>
      <ETag>"F2064A169EE92E9775EE5324D0B1682E"</ETag>
      <LastModified>2018-12-28T09:41:56.000Z</LastModified>
    </CopyObjectResult>
    ```

    **Note:** x-oss-hash-crc64ecma indicates the 64-bit CRC value of the object. This value is calculated based on the [ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm) standard. An object generated in a COPY operation may not have this value.


## SDK {#section_8o9_nxj_v5j .section}

The SDKs of this API are as follows:

-   [Java](../../../../reseller.en-US/SDK Reference/Java/Manage objects/Copy objects.md)
-   [Python](../../../../reseller.en-US/SDK Reference/Python/Manage objects/Copy objects.md)
-   [PHP](../../../../reseller.en-US/SDK Reference/PHP/Manage objects/Copy objects.md)
-   [Go](../../../../reseller.en-US/SDK Reference/Go/Manage objects/Copy objects.md)
-   [C](../../../../reseller.en-US/SDK Reference/C/Manage objects/Copy objects.md)
-   [.NET](../../../../reseller.en-US/SDK Reference/. NET/Manage objects/Copy objects.md#)
-   [iOS](../../../../reseller.en-US/SDK Reference/iOS/Manage objects/Overview.md)
-   [Node.js](../../../../reseller.en-US//Manage objects.md)
-   [Ruby](../../../../reseller.en-US/SDK Reference/Ruby/Manage objects.md)

## Error codes {#section_cv4_vtc_pxk .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidArgument|400|The values of parameters \(such as x-oss-storage-class are invalid.|
|Precondition Failed|412| -   The x-oss-copy-source-if-match header is specified in the request, but the provided ETag is different from the ETag of the source object.
-   The x-oss-copy-source-if-unmodified-since header is specified in the request, but the time specified in the request is earlier than the modification time of the object.

 |
|Not Modified|304| -   The x-oss-copy-source-if-none-match header is specified in the request, and the provided ETag is the same as the ETag of the source object.
-   The x-oss-copy-source-if-modified-since header is specified in the request, but the source object has not been modified after the time specified in the request.

 |
|KmsServiceNotEnabled|403|The x-oss-server-side-encryption header is set to KMS, but the KMS service is not enabled.|

