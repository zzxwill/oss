# ListParts {#reference_hzm_1zx_wdb .reference}

ListParts can be used to list all successfully uploaded parts mapped to a specific upload ID.

## Request syntax {#section_hsz_kzx_wdb .section}

```
Get /ObjectName? uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: Signature
```

## Request parameters {#section_wsj_nzx_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|uploadId|String|ID of a Multipart Upload event.Default: None

|
|max-parts|Integer|The maximum part number in the response of the OSS.Default: 1,000

|
|part-number-marker|Integer|Starting position of a specific list. A part is listed only when the part number is greater than the value of this parameter.Default: None

 |
|encoding-type|String|Specify the encoding of the returned content and the encoding type. The Key adopts UTF-8 encoding, but the XML 1.0 Standard does not support parsing certain control characters, such as the characters with ASCII values from 0 to 10. In case that the Key contains control characters not supported by the XML 1.0 Standard, you can specify the encoding-type to encode the returned Key.Default: None

Optional value: url

|

## Response elements {#section_omr_mby_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|ListPartsResult|Container|The container that saves the result of the List Parts request.Sub-nodes: Bucket, Key, UploadId, PartNumberMarker, NextPartNumberMarker, MaxParts, IsTruncated, Part

Parent node: None

|
|Bucket|String|Bucket name.Parent node: ListPartsResult

 |
|EncodingType|String|Specify the encoding type for the returned result. If the encoding type is specified in the request, the Key is encoded in the returned result.Parent node: ListPartsResult

|
|Key|String|The object name.Parent node: ListPartsResult

|
|UploadId|String|ID of an Upload event.Parent node: ListPartsResult

 |
|PartNumberMarker|Integer|Starting position of the part numbers in the listing result.Parent node: ListPartsResult

|
|NextPartNumberMarker|Integer|If not all results are returned this time, the response request includes the NextPartNumberMarker element to indicate the value of PartNumberMarker in the next request.Parent node: ListPartsResult

 |
|MaxParts|Integer|The maximum part number in the returned request.Parent node: ListPartsResult

|
|IsTruncated|Enumerating strings|Whether the returned result list for List  Part is truncated. The “true” indicates that not all results are returned; “false” indicates that all results are returned.Valid values: true, false

Parent node: ListPartsResult

|
|Part|String|The container that saves part information.Sub-nodes: PartNumber, LastModified, ETag, Size

Parent node: ListPartsResult

|
|PartNumber|Integer|Part number.Parent node: ListPartsResult.Part

|
|LastModified|Date|Time when a part is uploaded.Parent node: ListPartsResult.part

|
|ETag|String|ETag value in the content of the uploaded part. Parent node: ListPartsResult.Part

 |
|Size|Integer|Size of the uploaded part.Parent node: ListPartsResult.Part

|

## Detail analysis {#section_bpx_qcy_wdb .section}

-   ListParts supports two request parameters: max-parts and part-number-marker.
-   The maximum value of the max-parts parameter is 1,000; its default value is also 1,000.
-   The results returned by OSS are listed in ascending order based on the part numbers.
-   The errors may occur in network transmission, it is not recommended that you use the result \(part number and ETag value\) of List Parts to generate the final part list of Complete Multipart.

## Example {#section_gp1_5cy_wdb .section}

**Request example:**

```
Get /multipart.data? uploadId=0004B999EF5A239BB9138C6227D69F95 HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Thu, 23 Feb 2012 07:13:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:4qOnUMc9UQWqkz8wDqD3lIsa9P8=
```

**Response example:**

```
HTTP/1.1 200 
Server: AliyunOSS
Connection: keep-alive
Content-length: 1221
Content-type: application/xml
x-oss-request-id: 106452c8-10ff-812d-736e-c865294afc1c
Date: Thu, 23 Feb 2012 07:13:28 GMT

<? xml version="1.0" encoding="UTF-8"? >
<ListPartsResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Bucket>multipart_upload</Bucket>
    <Key>multipart.data</Key>
    <UploadId>0004B999EF5A239BB9138C6227D69F95</UploadId>
    <NextPartNumberMarker>5</NextPartNumberMarker>
    <MaxParts>1000</MaxParts>
    <IsTruncated>false</IsTruncated>
    <Part>
        <PartNumber>1</PartNumber>
        <LastModified>2012-02-23T07:01:34.000Z</LastModified>
        <ETag>&quot;3349DC700140D7F86A078484278075A9&quot;</ETag>
        <Size>6291456</Size>
    </Part>
    <Part>
        <PartNumber>2</PartNumber>
        <LastModified>2012-02-23T07:01:12.000Z</LastModified>
        <ETag>&quot;3349DC700140D7F86A078484278075A9&quot;</ETag>
        <Size>6291456</Size>
    </Part>
    <Part>
        <PartNumber>5</PartNumber>
        <LastModified>2012-02-23T07:02:03.000Z</LastModified>
        <ETag>&quot;7265F4D211B56873A381D321F586E4A9&quot;</ETag>
        <Size>1024</Size>
    </Part>
</ListPartsResult>
```

