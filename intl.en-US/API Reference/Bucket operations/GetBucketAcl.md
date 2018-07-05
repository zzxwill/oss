# GetBucketAcl {#reference_hgp_psv_tdb .reference}

GetBucketAcl is used to obtain the access permissions for a bucket.

## Request syntax {#section_d1r_t2w_bz .section}

```
GET /? acl HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_iwr_52w_bz .section}

|Name|Type|Description|
|----|----|-----------|
|Accesscontrollist|container|Container used for storing the ACL informationParent node: AccessControlPolicy

|
|AccessControlPolicy|container|Specify the container that stores the Get Bucket ACL resultParent node: none

|
|Displayname|string|Name of the bucket owner. \(Currently it is consistent with the ID \) Parent node: AccessControlPolicy.Owner

|
|Grant|enumerative string |ACL permissions of the bucket The acl permission for the bucket.Valid values: private, public-read, and public-read-write 

Parent node: AccessControlPolicy.AccessControlList

|
|ID|string|User ID of the bucket ownerParent node: AccessControlPolicy.Owner

|
|Owner|container|Container used for saving the information about the bucket owner Parent node: AccessControlPolicy

|

## Detail analysis {#section_opt_w2w_bz .section}

Only the bucket owner can use the GetBucketAcl interface.

## Example {#section_gwr_x2w_bz .section}

**Request example:**

```
GET /? acl HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 04:11:23 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0FmgbrQ0=

```

**Response example:**

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

