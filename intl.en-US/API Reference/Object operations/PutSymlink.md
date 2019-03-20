# PutSymlink {#reference_qzz_qzw_wdb .reference}

Creates a symbol link directing to the target object. You can use the symbol link to access the target object.

## Request syntax {#section_ndm_5zw_wdb .section}

```
PUT /ObjectName?symlink HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-symlink-target: TargetObjectName
```

## Request headers {#section_ztg_wzw_wdb .section}

|Header|Type|Required|Description|
|:-----|:---|--------|:----------|
|x-oss-symlink-target|String|Yes|Indicates the target object that the symbolic link directs to.Valid value: The naming conventions are the same as those for objects.

**Note:** 

-   Similar to ObjectName, TargetObjectName must be URL-encoded.
-   The target object that a symbolic link directs to cannot be a symbolic link.

|
|x-oss-storage-class|String|No|Specifies the storage class of the target object.Valid values:

-   Standard
-   IA
-   Archive

Supported APIs: PutObject, InitMultipartUpload, AppendObject, PutObjectSymlink, and CopyObject

**Note:** 

-   We recommend that you do not set the storage class in PutObjectSymlink to IA or Archive because an IA or Archive object smaller than 64 KB is billed at 64 KB.
-   If you specify the value of x-oss-storage-class when uploading an object to a bucket, the storage class of the uploaded object is the specified value of x-oss-storage-class regardless of the storage class of the bucket. For example, if you specify the value of x-oss-storage-class to Standard when uploading an object to a bucket of the IA storage class, the storage class of the object is Standard.

|

## Detail analysis {#section_zn5_d1x_wdb .section}

-   When a symbolic link is created, the following checks are not performed:

    -   Whether the target object exists.
    -   Whether the storage class of the target object is valid.
    -   Whether the user has permission to access the target object.
    These checks are performed by APIs that access the target object, such as GetObject.

-   If the object that you want to add already exists and you can access the object, the existing object is overwritten by the added object and a 200 OK message is returned.
-   If a PutSymlink request carries a parameter with the x-oss-meta- prefix, the parameter is considered as user meta, such as x-oss-meta-location. An object can have multiple parameters with the x-oss-meta- prefix. However, the total size of all user meta cannot exceed 8 KB.

## Examples {#section_th3_m1x_wdb .section}

Request example:

```
PUT /link-to-oss.jpg? symlink HTTP/1.1 
Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
Cache-control: no-cache 
Content-Disposition: attachment;filename=oss_download.jpg 
Date: Tue, 08 Nov 2016 02:00:25 GMT 
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk= x-oss-symlink-target: oss.jpg
x-oss-storage-class: Standard
```

Response example:

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Tue, 08 Nov 2016 02:00:25 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 582131B9109F4EE66CDE56A5
ETag: "0A477B89B4602AA8DECB8E19BFD447B6"
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage objects/Manage a symbolic link.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Manage objects/Manage a symbolic link.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Manage objects/Manage a symbolic link.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Manage objects/Manage a symbolic link.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Manage objects/Manage a symbolic link.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage objects/Manage a symbolic link.md)

## Error codes {#section_nfp_nfc_5gb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidArgument|400|The value of x-oss-storage-class is invalid.|

