# GetObjectACL {#reference_lzc_24w_wdb .reference}

The GetObjectACL operation is used to obtain the permission to access an object in a bucket.

## Request syntax  {#section_pzf_h4w_wdb .section}

```
GET /ObjectName? acl HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_qrd_j4w_wdb .section}

|Name |Type |Description |
|:----|:----|:-----------|
|AccessControlList|Container|Container used for storing the ACL information Parent node: AccessControlPolicy

|
|AccessControlPolicy|Container|Container that stores the Get Object ACL result Parent node: None

|
|DisplayName|String|Name of the bucket owner \(Currently is consistent with the ID\)Parent node: AccessControlPolicy.Owner

|
|Grant|Enumerating string|The ACL permission of an objectValid values: private，public-read，public-read-write

Parent node: AccessControlPolicy.AccessControlList

|
|ID|String|User ID of the bucket owner  Parent node: AccessControlPolicy.Owner

|
|Owner|Container|Container used for saving the information about the bucket ownerParent node: AccessControlPolicy

|

## Detail analysis {#section_rgf_k4w_wdb .section}

-   Only the bucket owner can use Get Object ACL to obtain the ACL of an object in the bucket. If you are not the bucket owner and send a Get Object ACL request, the system returns the 403 Forbidden message. Error code: AccessDenied. The message displayed is: You do not have read acl permission on this object.
-   If a Get Object ACL request is sent but the ACL has never been set for the object, ObjectACL returned by OSS is default, indicating that  the ACL of this object is the same as the bucket ACL. That is, if the access permission of the bucket is private, the access permission of this object is also private; if the access permission of the bucket is public-read-write, the access permission of this object is also public-read-write.

## Example {#section_swy_3pw_wdb .section}

**Request example:**

```
GET /test-object? acl HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Apr 2015 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0FmgbrQ0=
```

**Response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Apr 2015 05:21:12 GMT
Content-Length: 253
Content-Tupe: application/xml
Connection: keep-alive
Server: AliyunOSS

<? xml version="1.0" ? >
<AccessControlPolicy>
    <Owner>
        <ID>00220120222</ID>
        <DisplayName>00220120222</DisplayName>
    </Owner>
    <AccessControlList>
        <Grant>public-read </Grant>
    </AccessControlList>
</AccessControlPolicy>
```

