# PutSymlink {#reference_qzz_qzw_wdb .reference}

PutSymlink is used to create a symbolic link pointing to the TargetObject on OSS. Users can use the symbolic link to access the TargetObject.

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
|x-oss-symlink-target|String|Indicates the target file that a symbolic link directs to.Valid value: the naming rules are the same as that of objects.

|

## Detail analysis {#section_zn5_d1x_wdb .section}

-   As with ObjectName, TargetObjectName must be URL-encoded.
-   The target file type of a symbolic link cannot be the symbolic link.
-   When creating a symbolic link,

    -   The interface does not check whether the target file exists.
    -   The interface does not check whether the target file type is valid.
    -   The interface does not check access to the target file. 
    The foregoing checks are deferred until GetObject must access the API of the target file.

-   If a file to be added already exists and you have the file access permission, the newly-added file overwrites the existing file, and the system returns 200 OK.
-   If the PutSymlink request carries a parameter prefixed with x-oss-meta-, the parameter is treated as user meta, for example, x-oss-meta-location. A single object can have multiple similar parameters, but the total size of all user meta cannot exceed 8 KB.
-   If the bucket type is Archive, you cannot call this interface; otherwise, the system returns Error 400 with the error code “OperationNotSupported”.

## Example {#section_th3_m1x_wdb .section}

**Request example:**

```
PUT /link-to-oss.jpg? symlink HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Cache-control: no-cache
Content-Disposition: attachment;filename=oss_download.jpg
Date: Tue, 08 Nov 2016 02:00:25 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk=
x-oss-symlink-target: oss.jpg
```

**Return example:**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Tue, 08 Nov 2016 02:00:25 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 582131B9109F4EE66CDE56A5
ETag: "0A477B89B4602AA8DECB8E19BFD447B6"
```

