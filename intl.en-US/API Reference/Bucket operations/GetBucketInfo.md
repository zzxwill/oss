# GetBucketInfo {#reference_rwk_bwv_tdb .reference}

GetBucketInfo operation is used to view the bucket information.

The information includes the following:

-   Create time
-   Internet access endpoint
-   Intranet access endpoint
-   Bucket owner information
-   Bucket ACL \(AccessControlList\)

## Request syntax {#section_qw4_lfw_bz .section}

```
GET /? bucketInfo HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_bkn_mfw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|BucketInfo|Container|The container that saves the bucket information content Sub-node: Bucket node

Parent node: none

|
|Bucket|Container|The container that saves the bucket specific information  Parent node: BucketInfo node

|
|CreationDate|time|The creation time of the bucket. Time format: 2013-07-31T10:56:21.000Z  Parent node: BucketInfo.Bucket

|
|ExtranetEndpoint|string|The Internet domain name that the bucket accesses Parent node: BucketInfo.Bucket

|
|IntranetEndpoint|string|The intranet domain name for accessing the bucket from ECS in the same regionParent node: BucketInfo.Bucket

|
|Location|string|The region of the data center that the bucket is located in  Parent node: BucketInfo.Bucket

|
|Name|string|The bucket nameParent node: BucketInfo.Bucket

|
|Owner|container|Container used for saving the information about the bucket owner. Parent node: BucketInfo.Bucket

|
|ID|string|User ID of the bucket owner. Parent node: BucketInfo.Bucket.Owner

|
|DisplayName|string|Name of the bucket owner \(the same as the ID currently\). Parent node: BucketInfo.Bucket.Owner

|
|AccessControlList|container|Container used for storing the ACL information Parent node: BucketInfo.Bucket

|
|Grant|enumerative string|ACL permissions of the bucket. Valid values: private, public-read, and public-read-write

Parent node: BucketInfo.Bucket.AccessControlList

|
|DataRedundancyType|enumerative string|The data redundancy type of the bucket.Valid values: LRSand ZRS

Parent node: BucketInfo.Bucket

 |

## Detail analysis {#section_cgv_sfw_bz .section}

-   If the bucket does not exist, error 404 is returned. Error code: NoSuchBucket.
-   Only the owner of a bucket can view the information of the bucket. If other users attempt to access the location information, the error 403 Forbidden with the error code: AccessDenied is returned.
-   The request can be initiated from any OSS endpoint.

## Example {#section_i2n_tfw_bz .section}

**Request example:**

```
Get /? bucketInfo HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Sat, 12 Sep 2015 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=

```

**Return example after the bucket information is obtained successfully:**

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

**Return example if the requested bucket information does not exist:**

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

**Return example if the requester has no access permission to the bucket information:**

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

