# PutObject {#reference_l5p_ftw_tdb .reference}

PutObject is used to upload files.

## Request syntax {#section_ald_lkw_bz .section}

```
PUT /ObjectName HTTP/1.1
Content-Length: ContentLength
Content-Type: ContentType
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request header {#section_y1z_lkw_bz .section}

|Parameter|Type|Description|
|---------|----|-----------|
|Authorization|String|Indicates that the request is authorized. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: none

|
|Cache-control|String|Specifies the Web page caching behavior when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: none

|
|Content-Disposition|String|Specifies the name of the object when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: none

|
|Content-Encoding|String|Specifies the content encoding format when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: none

|
|Content-Md5|String|As defined in RFC 1864, an MD5 value, which is a 128-bit number, is calculated based on the message content, rather than the header. This number is then Base64-encoded into a Content-MD5 value. This request header can be used to check the validity of a message. More specifically, the name can be used whether the message content is consistent with the sent content. Although this request header is optional, we recommend that you use this request header for end-to-end checks. Default value: none

Restriction: none

|
|Content-Length|String|If the value of Content-Length in the request header is smaller than the data length in the request body, OSS can still create the object successfully. However, the object size is the value of Content-Length, and the data that exceeds the value is discarded.|
|ETag|String|An entity tag \(ETag\) is created to identify the content of an object when the object is created. For an object created with the PutObject request, its ETag is the MD5 value of the object content. For an object created by using other methods, its ETag is the UUID of the object content. The ETag value of an object can be used to check whether the object content has changed. However, we recommend that you not use the ETag of an object as the MD5 value of the object to verify data integrity.Default value: None

|
|Expires|String|Specify the expiration time. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: none

**Note:** OSS does not limit and verify this value.

|
|x-oss-server-side-encryption|String|Specifies the server-side encryption algorithm when OSS creates an object. Valid value: AES256 or KMS

**Note:** You must enable KMS \(Key Management Service\) in the console before you can use the KMS encryption algorithm. Otherwise, a KmsServiceNotEnabled error code is reported.

|
|x-oss-server-side-encryption-key-id|String|Specifies the primary key managed by KMS.This parameter is valid when the value of x-oss-server-side-encryption is set to KMS.

|
|x-oss-object-acl|String|Specifies the access permission when OSS creates an object. Valid values: public-read, private, and public-read-write

|
|x-oss-storage-class|String|Specifies the storage class of the object.Valid values:

-   Standard
-   IA
-   Archive

Supported interfaces: PutObject, InitMultipartUpload, AppendObject, PutObjectSymlink, and CopyObject

**Note:** 

-   The status code of 400 Bad Request is returned if the value of StorageClass is invalid. Error description: InvalidArgument.
-   If you specify the value of x-oss-storage-class when uploading an object to a bucket, the storage class of the uploaded object is the specified value of x-oss-storage-class. For example, if you specify the value of x-oss-storage-class to Standard when uploading an object to a bucket of the IA storage class, the storage class of the object is Standard.

|

## Detail analysis {#section_fcx_vkw_bz .section}

-   For the object that you want to upload:
    -   A status code 403 Forbidden error is returned if you do not have access to the bucket. Error description: AccessDenied.
    -   If an object with the same name already exists and you have access to it, the existing object is overwritten by the uploaded object, and the status code 200 OK is returned.
    -   The status code 404 Not Found is returned if the bucket does not exist. Error description: NoSuchBucket.
-   The status code 400 Bad Request is returned if the length of the input object key exceeds 1023 bytes. Error description: InvalidObjectName.
-   Content-Length
    -   If the value of Content-Length in the request header is smaller than the data length in the request body, OSS can still create the object successfully. However, the object size is the value of Content-Length, and the data that exceeds the value is discarded.
    -   The status code 400 Bad request is returned if the length of the uploaded object exceeds 5 GB. Error description: InvalidArgument.
    -   If the length is set, but the message body is not sent or the size of the sent body is smaller than the specified size, the server waits until timeout, and the status code 400 Bad Request is returned. Error description: RequestTimeout.
-   HTTP header
    -   OSS supports the following five header fields defined in HTTP: Cache-Control, Expires, Content-Encoding, Content-Disposition, and Content-Type. If these headers are set when you upload an object, the header values are automatically set to the values set in the upload when the object is downloaded. For more information about the header fields, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt).
    -   If the header is not encoded in the [chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1) method and the Content-Length parameter is not added, a 411 Length Required error is returned. Error decription: MissingContentLength.
    -   If you specify the x-oss-server-side-encryption header when you perform a PutObject operation, the value of this header must be set to AES256. Otherwise, the status code 400 Bad Request is returned. Error description: InvalidEncryptionAlgorithmError. After this header is specified, it is also included in the response header, and OSS encrypts the uploaded object by using the specified method. When this object is downloaded, x-oss-server-side-encryption is included in the response header, and the value of x-oss-server-side-encryption is set to the encryption algorithm of this object.
    -   If the PutObject request carries a parameter with the x-oss-meta- prefix, the parameter is considered as user meta, for example, x-oss-meta-location. A single object can have multiple parameters with the x-oss-meta- prefix. However, the total size of all user meta cannot exceed 8 KB. User meta can contain alphanumeric characters, en dashes \(–\), spaces \( \), and double quotation marks \("\). Other characters including underscores \(\_\) are not supported.

## Content-MD5 calculation error {#section_w35_wkw_bz .section}

According to the standard, the Content-MD5 value is calculated as follows: Calculate the MD5-encrypted 128-bit binary array, and then encode the binary array \(but not the 32-bit string\) with Base64.

For example, if the content you want to upload is `0123456789`, the Content-MD5 value of the string can be calculated as follows:

The correct calculation method can be implemented in Python as follows:

```

>>> import base64,hashlib
>>> hash = hashlib.md5()
>>> hash.update("0123456789")
>>> base64.b64encode(hash.digest())
'eB5eJF1ptWaXm4bijSPyxw=='
```

**Note:** 

Correct calculation method: Use `hash.digest()` to calculate the 128-bit binary array first. For example: `>>> hash.digest() 'x\x1e^$]i\xb5f\x97\x9b\x86\xe2\x8d#\xf2\xc7'`

A common error in calculations is to encode the calculated 32-bit string with Base64 to obtain the Content-MD5 value. For example, `hash.hexdigest()` is used to calculate a visible 32-bit string. `>>> hash.hexdigest() '781e5e245d69b566979b86e28d23f2c7'` If you encode the incorrect MD5 value with Base64, the result is as follows. `>>> base64.b64encode(hash.hexdigest()) 'NzgxZTVlMjQ1ZDY5YjU2Njk3OWI4NmUyOGQyM2YyYzc='`

## Examples { .section}

-   Example 1

    Request example in a simple upload:

    ```
    PUT /test.txt HTTP/1.1
    Host: test.oss-cn-zhangjiakou.aliyuncs.com
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: */*
    Connection: keep-alive
    Content-Type: text/plain
    date: Tue, 04 Dec 2018 15:56:37 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk=
    Transfer-Encoding: chunked
    ```

    Response example:

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 04 Dec 2018 15:56:38 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5C06A3B67B8B5A3DA422299D
    ETag: "D41D8CD98F00B204E9800998ECF8427E"
    x-oss-hash-crc64ecma: 0
    Content-MD5: 1B2M2Y8AsgTpgAmY7PhCfg==
    x-oss-server-time: 7
    ```

-   Example 2

    Request example in which the storage class is Archive:

    ```
    PUT /oss.jpg HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com Cache-control: no-cache 
    Expires: Fri, 28 Feb 2012 05:38:42 GMT 
    Content-Encoding: utf-8
    Content-Disposition: attachment;filename=oss_download.jpg 
    Date: Fri, 24 Feb 2012 06:03:28 GMT 
    Content-Type: image/jpg 
    Content-Length: 344606 
    x-oss-storage-class: Archive
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk=  
    
    [344606 bytes of object data]
    ```

    Response example:

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Sat, 21 Nov 2015 18:52:34 GMT
    Content-Type: image/jpg
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5650BD72207FB30443962F9A
    x-oss-bucket-version: 1418321259
    
    ETag: "A797938C31D59EDD08D86188F6D5B872"
    ```

-   **Note:** 

For more example code for PutObject, see [Upload objects](../../../../../reseller.en-US/SDK Reference/Python/Upload objects/Overview.md#).


