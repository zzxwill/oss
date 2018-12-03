# PutSymlink {#reference_qzz_qzw_wdb .reference}

PutSymlink is used to create a symbolic link directing to the TargetObject on OSS. You can use the symbolic link to access the TargetObject.

## Request syntax {#section_ndm_5zw_wdb .section}

```
PUT /ObjectName? symlink HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-symlink-target: TargetObjectName
```

## Request header {#section_ztg_wzw_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|x-oss-symlink-target|String|Indicates the target object that a symbolic link directs to.Valid value: The naming conventions are the same as those for objects.

|
|x-oss-storage-class|String|Specifies the storage class of the target object.Values:

-   Standard
-   IA
-   Archive

Supported interfaces: PutObject, InitMultipartUpload, AppendObject, PutObjectSymlink, and CopyObject

**Note:** 

-   If the value of StorageClass is invalid, a 400 error is returned. Error code: InvalidArgument
-   We recommend that you do not set the storage class in PutObjectSymlink to IA or Archive because an IA or Archive object smaller than 64 KB is billed at 64 KB.
-   If you specify the value of x-oss-storage-class when uploading an object to a bucket, the storage class of the uploaded object is the specified value of x-oss-storage-class regardless of the storage class of the bucket. For example, if you specify the value of x-oss-storage-class to Standard when uploading an object to a bucket of the IA storage class, the storage class of the object is Standard.

|

## Detail analysis {#section_zn5_d1x_wdb .section}

-   Similar to ObjectName, TargetObjectName must be URL-encoded.
-   The target object that a symbolic link directs to cannot be a symbolic link.
-   When a symbolic link is created, the following checks are not performed:

    -   Whether the target object exists.
    -   Whether the storage class of the target object is valid.
    -   Whether the user has permission to access the target object.
    These checks are performed by APIs that access the target object, such as GetObject.

-   If the object that you want to add already exists and you can access the object, the existing object is overwritten by the added object and a 200 OK message is returned.
-   If a PutSymlink request carries a parameter with the x-oss-meta- prefix, the parameter is considered as user meta, such as x-oss-meta-location. An object can have multiple parameters with the x-oss-meta- prefix. However, the total size of all user meta cannot exceed 8 KB.

## Example {#section_th3_m1x_wdb .section}

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

