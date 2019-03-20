# ListParts {#reference_hzm_1zx_wdb .reference}

Lists all parts that are successfully uploaded in a MultipartUpload event with a specified Upload ID.

## Request syntax {#section_hsz_kzx_wdb .section}

```
Get  /ObjectName?uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: Signature
```

## Request parameters {#section_wsj_nzx_wdb .section}

|Parameter|Type|Description|
|:--------|:---|:----------|
|uploadId|String|Specifies the ID of a MultipartUpload event.Default value: None

|
|max-parts|Integer|Specifies the maximum part number in the response.Default value: 1000

|
|part-number-marker|Integer|Specifies the starting position of a specified list. A part is listed only when the part number is greater than the value of this parameter.Default value: None

 |
|encoding-type|String|Indicates that the returned results are encoded and specifies the encoding type. The Key are UTF-8 encoded, but the XML 1.0 Standard does not support parsing certain control characters, such as the characters with ASCII values from 0 to 10. In case that the Key contains control characters not supported by the XML 1.0 Standard, you can specify the encoding-type to encode the returned Key.Default value: None

Optional value: url

|

## Response elements {#section_omr_mby_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|ListPartsResult|Container|Indicates the container that stores the result of the ListParts request.Sub-nodes: Bucket, Key, UploadId, PartNumberMarker, NextPartNumberMarker, MaxParts, IsTruncated, Part

Parent node: None

|
|Bucket|String|Indicates the bucket name.Parent node: ListPartsResult

 |
|EncodingType|String|Indicates the encoding type of the returned result. If the encoding type is specified in the request, the Key is encoded in the returned result.Parent node: ListPartsResult

|
|Key|String|Indicates the object name.Parent node: ListPartsResult

|
|UploadId|String|Indicates the ID of an MultipartUpload event.Parent node: ListPartsResult

 |
|PartNumberMarker|Integer|Indicates the starting position of the part numbers in the listing result.Parent node: ListPartsResult

|
|NextPartNumberMarker|Integer|If not all results are returned this time, the response request includes the NextPartNumberMarker element to indicate the value of PartNumberMarker in the next request.Parent node: ListPartsResult

 |
|MaxParts|Integer|Indicates the maximum part number in the returned result.Parent node: ListPartsResult

|
|IsTruncated|Enumerating strings|Indicates whether the returned result for the ListParts request is truncated. The “true” value indicates that not all results are returned; “false” indicates that all results are returned.Values: true, false

Parent node: ListPartsResult

|
|Part|String|Indicates the container that stores part information.Sub-nodes: PartNumber, LastModified, ETag, Size

Parent node: ListPartsResult

|
|PartNumber|Integer|Indicates the part number.Parent node: ListPartsResult.Part

|
|LastModified|Date|Indicates the time when a part is uploaded.Parent node: ListPartsResult.part

|
|ETag|String|Indicates the ETag value in the content of the uploaded part. Parent node: ListPartsResult.Part

 |
|Size|Integer|Indicates the size of the uploaded part.Parent node: ListPartsResult.Part

|

## Detail analysis {#section_bpx_qcy_wdb .section}

-   ListParts supports two request parameters: max-parts and part-number-marker.
-   The maximum value and the default value of the max-parts parameter are both 1,000.
-   The results returned by OSS are listed in ascending order based on the part numbers.
-   Errors may occur in network transmission. Therefore, we recommended you do not use the result \(part number and ETag value\) of ListParts to generate the final part list of CompleteMultipart.

## Examples {#section_gp1_5cy_wdb .section}

Request example:

```
Get /multipart.data? uploadId=0004B999EF5A239BB9138C6227D69F95 HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Thu, 23 Feb 2012 07:13:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:4qOnUMc9UQWqkz8wDqD3lIsa9P8=
```

Response example:

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

