# GetObjectACL {#reference_lzc_24w_wdb .reference}

Obtains the ACL for an object in a bucket.

**Note:** If the ACL for an object has not been set, the ObjectACL in the response to the GetObjectACL request is default, which indicates that the ACL for the object is the same as that for the bucket. For example, if the ACL for the bucket is private, the ACL for the object is also private.

## Request syntax  {#section_pzf_h4w_wdb .section}

```
GET /ObjectName?acl HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_qrd_j4w_wdb .section}

|Element|Type |Description |
|:------|:----|:-----------|
|AccessControlList|Container|Specifies the container used to store the ACL information.Parent node: AccessControlPolicy

|
|AccessControlPolicy|Container|Specifies the container that stores the returned result of the GetObjectACL request.Parent node: None

|
|DisplayName|String|Indicates the name of the bucket owner, which is the same as the value of ID.Parent node: AccessControlPolicy.Owner

|
|Grant|Enumerated string|Indicates the ACL for the object.Valid values: private, public-read, and public-read-write

Parent node: AccessControlPolicy.AccessControlList

|
|ID|String|Indicates the user ID of the bucket owner.Parent node: AccessControlPolicy.Owner

|
|Owner|Container|Specifies the container used to store the information about the bucket owner.Parent node: AccessControlPolicy

|

## Examples {#section_swy_3pw_wdb .section}

Request example:

```
GET /test-object? acl HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Apr 2015 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0FmgbrQ0=
```

Response example:

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

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage objects/Manage ACL for an object.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Manage objects/Manage ACL for an object.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Manage objects/Manage ACL for an object.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Manage objects/Manage ACL for an object.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage objects/Manage ACL for an object.md)

## Error codes {#section_nfp_nfc_5gb .section}

|Error code|HTTP Status code|Error message|Description|
|:---------|:---------------|:------------|:----------|
|AccessDenied|403|You do not have read acl permission on this object.| You do not have the permission to perform the GetObjectACL operation. Only the bucket owner can call GetObjectACL to obtain the ACL for an object in the bucket.

 |

