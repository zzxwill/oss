# GetBucketInfo {#reference_rwk_bwv_tdb .reference}

Views the information about a bucket. Only the owner of a bucket can view the information about the bucket.

**Note:** A GetBucketInfo request can be initiated from any OSS endpoint.

## Request syntax {#section_qw4_lfw_bz .section}

```
GET /? bucketInfo HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_bkn_mfw_bz .section}

|Element|Type|Description|
|-------|----|-----------|
|BucketInfo|Container|Indicates the container that stores the bucket information.Sub-node: Bucket

Parent node: None

|
|Bucket|Container|Indicates the container that stores specific bucket information.Parent node: BucketInfo

|
|CreationDate|Time|Indicates the time when the bucket is created.Parent node: BucketInfo.Bucket

|
|ExtranetEndpoint|String|Indicates the domain name used to access the bucket through the Internet.Parent node: BucketInfo.Bucket

|
|IntranetEndpoint|String|Indicates the domain name used by the ECS instances in the same region to access the bucket through the intranet.Parent node: BucketInfo.Bucket

|
|Location|String|Indicates the region where the bucket is located. Parent node: BucketInfo.Bucket

|
|Name|String|Indicates the bucket name.Parent node: BucketInfo.Bucket

|
|Owner|Container|Indicates the container used to store the information about the bucket owner.Parent node: BucketInfo.Bucket

|
|ID|String|Indicates the user ID of the bucket owner. Parent node: BucketInfo.Bucket.Owner

|
|DisplayName|String|Indicates the name of the bucket owner, which is the same as the value ID.Parent node: BucketInfo.Bucket.Owner

|
|AccessControlList|Container|Indicates the container used to store the ACL information.Parent node: BucketInfo.Bucket

|
|Grant|Enumerated string|Indicates the ACL for the bucket. Valid values: private, public-read, and public-read-write

Parent node: BucketInfo.Bucket.AccessControlList

|
|DataRedundancyType|Enumerated string|Indicates the data redundancy type of the bucket.Valid values: LRS and ZRS

Parent node: BucketInfo.Bucket

 |
|StorageClass|String|Indicates the storage class of the bucket.Valid value: Standard, IA, and Archive

|

## Examples {#section_i2n_tfw_bz .section}

Request example:

```
Get /? bucketInfo HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Sat, 12 Sep 2015 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=

```

Response example returned when the bucket information is obtained successfully:

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Sat, 12 Sep 2015 07:51:28 GMT
Connection: keep-alive
Content-Length: 531  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<BucketInfo>
  <Bucket>
    <CreationDate>2013-07-31T10:56:21.000Z</CreationDate>
    <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
    <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
    <Location>oss-cn-hangzhou</Location>
    <Name>oss-example</Name>
    <Owner>
      <DisplayName>username</DisplayName>
      <ID>271834739143143</ID>
    </Owner>
    <AccessControlList>
      <Grant>private</Grant>
    </AccessControlList>
  </Bucket>
</BucketInfo>
```

Response example returned when the requested bucket does not exist:

```
HTTP/1.1 404 
x-oss-request-id: 534B371674E88A4D8906009B
Date: Sat, 12 Sep 2015 07:51:28 GMT
Connection: keep-alive
Content-Length: 308  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>NoSuchBucket</Code>
  <Message>The specified bucket does not exist.</Message>
  <RequestId>568D547F31243C673BA14274</RequestId>
  <HostId>nosuchbucket.oss.aliyuncs.com</HostId>
  <BucketName>nosuchbucket</BucketName>
</Error>
```

Response example returned when the requester has no access permission to the bucket:

```
HTTP/1.1 403
x-oss-request-id: 534B371674E88A4D8906008C
Date: Sat, 12 Sep 2015 07:51:28 GMT
Connection: keep-alive
Content-Length: 209  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>AccessDenied</Code>
  <Message>AccessDenied</Message>
  <RequestId>568D5566F2D0F89F5C0EB66E</RequestId>
  <Hostid> test.oss.aliyuncs.com </hostid>
</Error>
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage a bucket.md)
-   [Python](../../../../../intl.en-US/SDK Reference/Python/Bucket.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Bucket.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Bucket.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Bucket.md)

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The target bucket does not exist.|
|AccessDenied|403|You do not have the permission to view the bucket information. Only the owner of a bucket can view the information about the bucket.|

