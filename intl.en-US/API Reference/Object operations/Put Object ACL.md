# Put Object ACL {#reference_fs3_gfw_wdb .reference}

The Put Object ACL interface is used to modify the access permission of an object.

Currently, an object may have four types of access permissions: default, private, public-read, and public-read-write. You can use the "x-oss-object-acl" header in the Put Object ACL request to set the access permission. Only the bucket owner has the permission to perform this operation. If the operation succeeds, 200 is returned; otherwise, the corresponding error code and prompt message are returned.

## Request syntax {#section_ign_5fw_wdb .section}

```
PUT /ObjectName? acl HTTP/1.1
x-oss-object-acl: Permission
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Definition of Object ACL {#section_bg5_wfw_wdb .section}

|Name|Description|
|:---|:----------|
|private|This ACL indicates that an object is a private resource. Only the owner of this object has the permission to read or write this object.|
|public-read|This ACL indicates that an object is a resource that can be read by the public. Only the owner of this object  has the permission to read and write this object. Other users only have the permission to read this object.|
|public-read-write|This ACL indicates that an object is a resource that can be read and written by the public. All users have the permission to read and write this object.|
|default|This ACL indicates an object is a resource inheriting the read-write permissions of the bucket. That is, the bucket and the object have the same permissions.|

## Detail analysis {#section_ik1_zfw_wdb .section}

-   Read operations to an object include: the read operations to the source object in GetObject, HeadObject, CopyObject, and UploadPartCopy. Write operations to an object include: the write operations to a new object in PutObject, PostObject, AppendObject, DeleteObject, DeleteMultipleObjects, CompleteMultipartUpload, and CopyObject.
-   The x-oss-object-acl must be set to one of the preceding four permissions. Otherwise, OSS returns the 400 Bad Request message and the error code is: InvalidArgument.
-   You can use PutObject ACL to set the ACL of an object. In addition, when writing an object, you can include x-oss-object-acl in the request header to set the ACL of the object. The effect is equivalent to PutObject ACL. For example, if the header of the PutObject request carries x-oss-object-acl, you can set the ACL of an object while uploading the object.
-   When a user who has no permission to read an object reads the object, OSS returns the 403 Forbidden message and the error code is: AccessDenied. The message displayed is: You do not have read permission on this object.
-   When a user who has no permission to write an object writes the object, OSS returns the 403 Forbidden message and the error code is: AccessDenied. The message displayed is: You do not have write permission on this object.
-   Only the owner of a bucket has the permission to call the PutObject ACL to modify the ACL for an object in this bucket. When a non-bucket owner calls the PutObject ACL, OSS returns the 403 Forbidden message and the error code is: AccessDenied. The message displayed is: You do not have write acl permission on this object.
-   The object ACL takes precedence over the bucket ACL. For example, if the bucket ACL is private and the object ACL is public-read-write, the system first checks the ACL of the object when a user accesses the object. As a result, all users can access this object even if the bucket is a private bucket. If the ACL of an object has never been set, the ACL of this object is same as that of the bucket where the object is located.

## Example {#section_awy_3gw_wdb .section}

**Request example:**

```
Put/test-object? acl HTTP/1.1
x-oss-object-acl: public-read
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Apr 2015 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
```

**Response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Apr 2015 05:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

