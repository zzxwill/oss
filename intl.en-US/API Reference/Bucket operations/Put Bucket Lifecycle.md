# Put Bucket Lifecycle {#reference_xlw_dbv_tdb .reference}

The bucket owner can set the lifecycle of a bucket with the Put Bucket Lifecycle request.  After Lifecycle is enabled, OSS automatically deletes the objects or transitions the objects \(to another storage class\) corresponding the lifecycle rules on a regular basis.

## Request syntax {#section_bfn_rcw_bz .section}

```
PUT /? lifecycle HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Authorization: SignatureValue 
Host: BucketName.oss.aliyuncs.com
<? xml version="1.0" encoding="UTF-8"? >
<LifecycleConfiguration>
  <Rule>
    <ID>RuleID</ID>
    <Prefix>Prefix</Prefix>
    <Status>Status</Status>
    <Expiration>
      <Days>Days</Days>
    </Expiration>
    <Transition>
      <Days>Days</Days>
      <StorageClass>StorageClass</StorageClass>
    </Transition>
    <AbortMultipartUpload>
      <Days>Days</Days>
    </AbortMultipartUpload>
  </Rule>
</LifecycleConfiguration>
```

## Request elements {#section_vrs_vcw_bz .section}

|Name|Type|Required?|Description|
|----|----|---------|-----------|
|CreatedBeforeDate|string |One from the two: Days and CreatedBeforeDate|Specify the time before which the rules will be effective.  The date must conform to the ISO8601 format and always be UTC 00:00.  For example: 2002-10-11T00:00:00.000Z,  which means the objects with a last modification time before 2002-10-11T00:00:00.000Z are deleted or transitioned to another storage class, and the objects modified after this time are not deleted or transitioned.  Parent node: Expiration or AbortMultipartUpload

|
|Days|positive integer |One from the two: Days and CreatedBeforeDate|Specify how many days after the last object modification until the rules take effect.  Parent node: Expiration

|
|Expiration|container |No|Specify the expiration attribute of the object.  Sub-node: Days or CreatedBeforeDate 

Parent node: Rule

|
|AbortMultipartUpload|container |No|Specify the expiration attribute of the unfulfilled Part rules.  Sub-node: Days or CreatedBeforeDate 

Parent node: Rule

|
|ID|string|No|The unique ID of a rule.   An ID is composed of 255 bytes at most.  If this value is not specified or this value is null, OSS generates a unique value for you.  Sub-node: none 

Parent node: Rule

|
|LifecycleConfiguration|container|Yes|Container used for storing lifecycle configurations, which can hold a maximum of 1000 rules.  Sub-node: Rule 

Parent node: none

|
|Prefix|string|Yes|Specify the prefix applicable to a rule.  Only those objects with a matching prefix can be affected by the rule.  It cannot be overlapped.  Sub-node: none 

Parent node: Rule

|
|Rule|container|Yes|Express a rule  Sub-nodes: ID, Prefix, Status, Expiration 

Parent node: LifecycleConfiguration

|
|Status|string|Yes|If this value is Enabled, OSS runs this rule regularly. If this value is Disabled, then OSS ignores this rule.  Parent node: Rule 

Valid value: Enabled, Disabled

|
|StorageClass|string|Required if parent node transition is set|Specifies the type of target storage that the object is transition to the OSS. Value:-   IA
-   Archive

Parent node: Transition

 |
|Transition|Container|No|Specifies when the object is transition to the IA or archive storage type during a valid life cycle.|

## Detail analysis {#section_udl_ycw_bz .section}

-   Only the bucket owner can initiate a Put Bucket Lifecycle request. Otherwise, the message of 403 Forbidden is returned.  Error code: AccessDenied.
-   If no lifecycle has been set previously, this operation creates a new lifecycle configuration or overwrites the previous configuration.
-   You can also set an expiration time for an object, or for the Part.  Here, the Part refers to the unsubmitted parts for multipart upload.

Notes for storage types transition:

-   Supports standard objects transition to IA, archive storage type, standard in the standard Bucket Bucket can configure both transition to IA and archive storage type rules for an object, configure both transition to IA and archive types, the time to transition to archive must be longer than the time to transition to IA. Such as transition IA sets days to 30, transition archive sets days must be greater than IA. Otherwise, the invalidargument error is returned.
-   The object setting must have an expiration time greater than the time converted to IA or archive. Otherwise, the invalidArgument error is returned.
-   Supports objects transition to archive storage type in IA bucket.
-   Archvie bucket creation is not supported.
-   IA object convertion is not supported as standard.
-   The archive object convertion is not supported for IA or standard.

## Examples {#section_ox3_zcw_bz .section}

**Request example:**

```
PUT /? lifecycle HTTP/1.1
Host: oss-example.oss.aliyuncs.com
Content-Length: 443
Date: Thu , 8 Jun 2017 13:08:38 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:PYbzsdWSMrAIWAlMW8luWekJ8=
<? xml version="1.0" encoding="UTF-8"? >
<LifecycleConfiguration>
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
    <ID>transit objects to IA after 30, to Archive 60, expire after 10 years</ID>
    <Prefix>data/</Prefix>
    <Status>Enabled</Status>
    <Transition>
      <Days>30</Days>
      <StorageClass>IA</StorageClass>
    </Transition>
    <Transition>
      <Days>60</Days>
      <StorageClass>Archive</StorageClass>
    </Transition>
    <Expiration>
      <Days>3600</Days>
    </Expiration>
  </Rule>
  <Rule>
    <ID>transit objects to Archive after 60 days</ID>
    <Prefix>important/</Prefix>
    <Status>Enabled</Status>
    <Transition>
      <Days>6</Days>
      <StorageClass>Archive</StorageClass>
    </Transition>
  </Rule>
  <Rule>
    <ID>delete created before date</ID>
    <Prefix>backup/</Prefix>
    <Status>Enabled</Status>
    <Expiration>
      <CreatedBeforeDate>2017-01-01T00:00:00.000Z</CreatedBeforeDate>
    </Expiration>
    <AbortMultipartUpload>
      <CreatedBeforeDate>2017-01-01T00:00:00.000Z</CreatedBeforeDate>
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

