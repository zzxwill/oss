# PutObjectACL {#reference_fs3_gfw_wdb .reference}

Modifies the ACL for an object. Only the bucket owner who has the write permission on the requested object can perform PutObjectACL operations.

**Note:** 

-   The object ACL takes precedence over the bucket ACL. For example, if the bucket ACL is private and the object ACL is public-read-write, OSS first checks the ACL for the object when a user accesses the object. As a result, all users can access this object even if the ACL for the bucket is a private. If the ACL for an object has never been set, the ACL for this object is same as that for the bucket where the object is located.
-   Read operations to an object include: the read operations to the source object in GetObject, HeadObject, CopyObject, and UploadPartCopy Write operations to an object include: the write operations on a new object in PutObject, PostObject, AppendObject, DeleteObject, DeleteMultipleObjects, CompleteMultipartUpload, and CopyObject.
-   You can also include the x-oss-object-acl header in the request to set the ACL for an object when performing write operations on the object. For example, if you include the x-oss-object-acl header in the PutObject request, you can set the ACL for the object while uploading it.

## ACL overview {#section_bg5_wfw_wdb .section}

You can specify the x-oss-object-acl header in the PutObjectACL request.to set the ACL for an object. The following table describes the four ACLs that can be set for an object.

|ACL|Description|
|:--|:----------|
|private|This ACL indicates that an object is a private resource. Only the owner of this object has the permission to read or write this object.|
|public-read|This ACL indicates that an object is a resource that can be read by the public. Only the owner of this object  has the permission to read and write this object. Other users only have the permission to read this object.|
|public-read-write|This ACL indicates that an object is a resource that can be read and written by the public. All users have the permission to read and write this object.|
|default|This ACL indicates an object is a resource inheriting the read-write permissions of the bucket. That is, the bucket and the object have the same permissions.|

## Request syntax {#section_ign_5fw_wdb .section}

```
PUT /ObjectName? acl HTTP/1.1
x-oss-object-acl: Permission
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Examples {#section_awy_3gw_wdb .section}

Request example:

```
PUT /test-object?acl HTTP/1.1
x-oss-object-acl: public-read
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Apr 2015 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
```

Response example:

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Apr 2015 05:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage objects/Manage ACL for an object.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Manage objects/Manage ACL for an object.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Manage objects/Manage ACL for an object.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Manage objects/Manage ACL for an object.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Manage objects/Manage ACL for an object.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage objects/Manage ACL for an object.md)
-   [Node.js](../../../../../intl.en-US/SDK Reference/Node. js/Set ACLs.md)
-   [Browser.js](../../../../../intl.en-US/SDK Reference/Browser.js/Set ACLs.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Set ACLs.md)

## Error codes {#section_nfp_nfc_5gb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|AccessDenied|403|The user is not the bucket owner or does not have the read and write permissions on the object.|
|InvalidArgument|400|The value of x-oss-object-acl is invalid.|

