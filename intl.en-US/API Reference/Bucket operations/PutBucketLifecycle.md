# PutBucketLifecycle {#reference_xlw_dbv_tdb .reference}

The bucket owner can set the lifecycle of a bucket with the PutBucketLifecycle request. After Lifecycle is enabled, OSS automatically deletes the objects or transitions the objects \(to another storage class\) corresponding the lifecycle rules on a regular basis.

## Request syntax {#section_bfn_rcw_bz .section}

```
PUT /?lifecycle HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
Content-Type: application/xml
Authorization: SignatureValue 
Host: BucketName.oss.aliyuncs.com
<?xml version="1.0" encoding="UTF-8"?>
<LifecycleConfiguration>
  <Rule>
    <ID>RuleID</ID>
    <Prefix>Prefix</Prefix>
    <Status>Status</Status>
    <Expiration>
      <Days>Days</Days>
    </Expiration>
    <AbortMultipartUpload>
      <Days>Days</Days>
    </AbortMultipartUpload>
  </Rule>
</LifecycleConfiguration>
```

## Request elements {#section_vrs_vcw_bz .section}

|Name|Type|Required?|Description|
|----|----|---------|-----------|
|CreatedBeforeDate|string |One from the two: Days and CreatedBeforeDate|Specify the time before which the rules go into effect. The date must conform to the ISO8601 format and always be UTC 00:00. For example: 2002-10-11T00:00:00.000Z, which means the objects with a last update time before 2002-10-11T00:00:00.000Z are deleted or transitioned to another storage class, and the objects updated after this time are not deleted or transitioned. Parent node: Expiration or AbortMultipartUpload

|
|Days|positive integer |One from the two: Days and CreatedBeforeDate|Specify how many days after the last object update until the rules take effect.  Parent node: Expiration

|
|Expiration|container |No|Specify the expiration attribute of the object.  Sub-node: Days or CreatedBeforeDate 

Parent node: Rule

|
|AbortMultipartUpload|container |No|Specify the expiration attribute of the unfulfilled Part rules.  Sub-node: Days or CreatedBeforeDate 

Parent node: Rule

|
|ID|string|No|The unique ID of a rule. An ID is composed of 255 bytes at most. When you fail to specify this value or this value is null, OSS generates a unique value for you. Sub-node: none 

Parent node: Rule

|
|LifecycleConfiguration|container|Yes|Container used for storing lifecycle configurations, which can hold a maximum of 1000 rules.  Sub-node: Rule 

Parent node: none

|
|Prefix|string|Yes|Specify the prefix applicable to a rule. Only those objects with a matching prefix can be affected by the rule.  It cannot be overlapped.  Sub-node: none 

Parent node: Rule

|
|Rule|container|Yes|Express a rule  Sub-nodes: ID, Prefix, Status, Expiration 

Parent node: LifecycleConfiguration

|
|Status|string|Yes|If this value is Enabled, OSS runs this rule regularly. If this value is Disabled, then OSS ignores this rule.  Parent node: Rule 

Valid value: Enabled, Disabled

|
|StorageClass|string|Required if parent node transition is set|Specifies the type of target storage that the object is transition to the OSS. Value: IA, Archive

Parent node: Transition

 |
|Transition|Container|No|Specifies when the object is transition to the IA or archive storage type during a valid life cycle.|

## Detail analysis {#section_udl_ycw_bz .section}

-   Only the bucket owner can initiate a Put Bucket Lifecycle request. Otherwise, the message of 403 Forbidden is returned.  Error code: AccessDenied.
-   If no lifecycle has been set previously, this operation creates a new lifecycle configuration or overwrites the previous configuration.
-   You can also set an expiration time for an object, or for the Part. Here, the Part refers to the unsubmitted parts for multipart upload.

Notes for storage types transition:

-   Supports objects in Standard bucket transition to IA and Archive storage type. Standard bucket can simultaneously configure both transition to IA and archive storage type rules for one object. In this case, the time set to transition to archive must be longer than the time to transition to IA. For example, the days set for transition to IA is 30, then it must be greater than 30 days set for transition to archive. Otherwise, the invalidargument error is returned.
-   The object setting must have an expiration time greater than the time converted to IA or archive. Otherwise, the invalidArgument error is returned.
-   Supports objects transition to archive storage type in IA bucket.
-   Archvie bucket creation is not supported.
-   IA object convertion is not supported as standard.
-   The archive object convertion is not supported for IA or standard.

## Examples {#section_ox3_zcw_bz .section}

**Request example:**

```
PUT /?lifecycle HTTP/1.1
Host: oss-example.oss.aliyuncs.com
Content-Length: 443
Date: Mon, 14 Apr 2014 01:08:38 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
<?xml version="1.0" encoding="UTF-8"?>
</LifecycleConfiguration>
  <Rule>
    <ID>delete objects and parts after one day</ID>
    <Prefix>logs/</Prefix>
    <Status>Enabled</Status>
    <Expiration>
      <Days>1</Days>
    </Expiration>
    <AbortMultipartUpload>
      <Days>1</Days>
    </AbortMultipartUpload>
  </Rule>
  <Rule>
    <ID>delete created before date</ID>
    <Prefix>backup/</Prefix>
    <Status>Enabled</Status>
    <Expiration>
      <CreatedBeforeDate>2014-10-11T00:00:00.000Z</CreatedBeforeDate>
    </Expiration>
    <AbortMultipartUpload>
      <CreatedBeforeDate>2014-10-11T00:00:00.000Z</CreatedBeforeDate>
    </AbortMultipartUpload>
  </Rule>
</LifecycleConfiguration>
```

**Response example:**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu , 8 Jun 2017 13:08:38 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

