# GetService \(ListBuckets\) {#reference_ahf_k4t_tdb .reference}

Sending a Get request to the server can return all buckets owned by the requester, and “/“ represents the root directory.

## Request syntax {#section_glm_xkr_bz .section}

```
GET / HTTP/1.1
Host: oss.example.com
Date: GMT Date
Authorization: SignatureValue
```

## Request parameters {#section_z4x_zkr_bz .section}

When using GetService\(ListBuckets\), you can prescribe a limit to the list with a prefix, marker, and max-uploads to return partial results.

|Name|Type|Required|Description|
|----|----|--------|-----------|
|prefix|string|No| Indicates that only the buckets whose names match a specified prefix are returned. If this parameter is not specified, prefix information is not used as a filter.Default value: None

|
|marker|string|No|Indicates that the returned results start with the first entry after the marker in an alphabetical order. If this parameter is not specified, all entries are returned from the start.Default value: None

|
|max-keys|string|No|Limits the maximum number of buckets returned for one request. If this parameter is not specified, the default value 100 is used. The value cannot exceed 1000.Default value: 100

|

## Response elements {#section_fvy_clr_bz .section}

|Name|Type|Description|
|----|----|-----------|
|ListAllMyBucketsResult|container|Container for saving results of the Get Service request.Subnode: Owner and Buckets

Parent node: None

|
|Prefix|string|Prefix of the returned bucket names for one request. This node is available only when not all buckets are returned.Parent node: ListAllMyBucketsResult

|
|Marker|string|Start point of the current GetService\(ListBuckets\) request. This node is available only when not all buckets are returned.Parent node: ListAllMyBucketsResult

|
|Maxkeys|string|The maximum number of returned results for one request. This node is available only when not all buckets are returned.Parent node: ListAllMyBucketsResult

|
|IsTruncated|Enumerated string|Indicates whether all results have been returned. “true” means that not all results are returned this time; “false” means that all results are returned this time.  This node is available only when not all buckets are returned.Valid values: “true” and “false”

Parent node: ListAllMyBucketsResult

|
|NextMarker|string|To indicate that this can be counted as a marker for the next GetService\(ListBuckets\) request to return the unreturned results.  This node is available only when not all buckets are returned.Parent node: ListAllMyBucketsResult

|
|Owner|container|Container used for saving the information about the bucket owner.Parent node: ListAllMyBucketsResult

|
|ID|String|User ID of the bucket owner.Parent node: ListAllMyBucketsResult.Owner

|
|DisplayName|string|Name of the bucket owner \(the same as the ID currently\).Parent node: ListAllMyBucketsResult.Owner

|
|Buckets|container|Container used for saving the information about multiple Buckets.Subnode: Bucket

Parent node: ListAllMyBucketsResult

|
|Bucket|container|Container used for saving the bucket information. Subnodes: Name, CreationDate, and Location

Parent node: ListAllMyBucketsResult.Buckets

|
|Name|string|Bucket name.Parent node: ListAllMyBucketsResult.Buckets.Bucket

|
|CreateDate|time \(format: yyyy-mm-ddThh:mm:ss.timezone, for example, 2011-12-01T12:27:13.000Z\)|Bucket creation timeParent node: ListAllMyBucketsResult.Buckets.Bucket

|
|Location|string|Indicates the data center in which a bucket is locatedParent node: ListAllMyBucketsResult.Buckets.Bucket

|
|ExtranetEndpoint|string|Internet domain name accessed by the bucket  Parent node: ListAllMyBucketsResult.Buckets.Bucket

|
|IntranetEndpoint|string|Intranet domain name accessed by the ECS in the same region.Parent node: ListAllMyBucketsResult.Buckets.Bucket

|
|StorageClass|string|Indicates the bucket storage type. “Standard”, “IA”, and “Archive” types are available. \( The “Archive” type is only available in some regions currently.\)Parent node: ListAllMyBucketsResult.Buckets.Bucket

|

## Detail analysis {#section_ets_qlr_bz .section}

-   The API of GetService is valid only for those users who have been authenticated.
-   If no information for user authentication is provided in a request \(namely an anonymous access\), 403 Forbidden is returned.  The error code is “AccessDenied”.
-   When all buckets are returned, the returned XML does not contain the nodes Prefix, Marker, MaxKeys, IsTruncated, and NextMarker. If some results are not returned yet, the preceding nodes are added, in which NextMarker is used to assign the marker for the successive query.

## Example {#section_tm3_slr_bz .section}

**Request example I**

```
GET / HTTP/1.1
Date: Thu, 15 May 2014 11:18:32 GMT
Host: oss-cn-hangzhou.aliyuncs.com
Authorization: OSS nxj7dtl1c24jwhcyl5hpvnhi:COS3OQkfQPnKmYZTEHYv2qUl5jI=
```

**Return example I**

```
HTTP/1.1 200 OK
Date: Thu, 15 May 2014 11:18:32 GMT
Content-Type: application/xml
Content-Length: 556
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D74
<? xml version="1.0" encoding="UTF-8"? >
<ListAllMyBucketsResult>
  <Owner>
    <ID>51264</ID>
    <DisplayName>51264</DisplayName>
  </Owner>
  <Buckets>
    <Bucket>
      <CreationDate>2015-12-17T18:12:43.000Z</CreationDate>
      <ExtranetEndpoint>oss-cn-shanghai.aliyuncs.com</ExtranetEndpoint>
      <IntranetEndpoint>oss-cn-shanghai-internal.aliyuncs.com</IntranetEndpoint>
      <Location>oss-cn-shanghai</Location>
      <Name>app-base-oss</Name>
      <StorageClass>Standard</StorageClass>
    </Bucket>
    <Bucket>
      <CreationDate>2014-12-25T11:21:04.000Z</CreationDate>
      <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
      <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
      <Location>oss-cn-hangzhou</Location>
      <Name>atestleo23</Name>
      <StorageClass>IA</StorageClass>
    </Bucket>
  </Buckets>
</ListAllMyBucketsResult>
```

**Request example II**

```
GET /? prefix=xz02tphky6fjfiuc&max-keys=1 HTTP/1.1
Date: Thu, 15 May 2014 11:18:32 GMT
Host: oss-cn-hangzhou.aliyuncs.com
Authorization: OSS nxj7dtl1c24jwhcyl5hpvnhi:COS3OQkfQPnKmYZTEHYv2qUl5jI=
```

**Return example II**

```
HTTP/1.1 200 OK
Date: Thu, 15 May 2014 11:18:32 GMT
Content-Type: application/xml
Content-Length: 545
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D75
<? xml version="1.0" encoding="UTF-8"? >
<ListAllMyBucketsResult>
  <Prefix>xz02tphky6fjfiuc</Prefix>
  <Marker></Marker>
  <MaxKeys>1</MaxKeys>
  <IsTruncated>true</IsTruncated>
  <NextMarker>xz02tphky6fjfiuc0</NextMarker>
  <Owner>
    <ID>ut_test_put_bucket</ID>
    <DisplayName>ut_test_put_bucket</DisplayName>
  </Owner>
  <Buckets>
    <Bucket>
      <CreationDate>2014-05-15T11:18:32.000Z</CreationDate>
      <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
      <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
      <Location>oss-cn-hangzhou</Location>
      <Name>xz02tphky6fjfiuc0</Name>
      <StorageClass>Standard</StorageClass>
    </Bucket>
  </Buckets>
</ListAllMyBucketsResult>
```

