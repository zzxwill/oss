# GetBucketAcl {#reference_hgp_psv_tdb .reference}

Obtains the ACL for a bucket. Only the owner of a bucket can obtain the ACL for the bucket.

## Request syntax {#section_d1r_t2w_bz .section}

```
GET /? acl HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_iwr_52w_bz .section}

|Elements|Type|Description|
|--------|----|-----------|
|Accesscontrollist|Container|Specifies the container used to store the ACL information.Parent node: AccessControlPolicy

|
|AccessControlPolicy|Container|Specifies the container that stores the result to the GetBucketACL request.Parent node: None

|
|Displayname|String|Indicates the name of the bucket owner, which is the same as the value of ID. Parent Node: AccessControlPolicy.Owner

|
|Grant|Enumerated string |Indicates the ACL for the bucket.Valid values: private, public-read, and public-read-write

Parent node: AccessControlPolicy.AccessControlList

|
|ID|String|Indicates the user ID of the bucket owner.Parent node: AccessControlPolicy.Owner

|
|Owner|Container|Indicates the container used to store the information about the bucket owner.Parent node: AccessControlPolicy

|

## Examples {#section_gwr_x2w_bz .section}

Request example:

```
GET /? acl HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 04:11:23 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0FmgbrQ0=

```

Response example:

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 04:11:23 GMT 
Content-Length: 253
Content-Type: application/xml
Connection: keep-alive
Server: AliyunOSS

<? xml version="1.0" ? >
<AccessControlPolicy>
    <Owner>
        <ID>00220120222</ID>
        <DisplayName>user_example</DisplayName>
    </Owner>
    <AccessControlList>
        <Grant>public-read</Grant>
    </AccessControlList>
</AccessControlPolicy>
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage a bucket.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Bucket.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Bucket.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Bucket.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Bucket.md)
-   [.NET](../../../../../intl.en-US/SDK Reference/. NET/Manage a bucket.md)
-   [Node.js](../../../../../intl.en-US/SDK Reference/Node. js/Manage a bucket.md)
-   [Ruby](../../../../../intl.en-US/SDK Reference/Ruby/Manage buckets.md)

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The target bucket does not exist.|
|AccessDenied|403|You do not have the permission to perform this operation. Only the owner of a bucket can obtain the ACL for the bucket.|

