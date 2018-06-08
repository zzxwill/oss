# CompleteMultipartUpload {#reference_lq1_dtx_wdb .reference}

After uploading all data parts, you must call the Complete Multipart Upload API to complete Multipart Upload for the entire file.

During this operation, you must provide the list \(including the part number and ETags\) of all valid data parts. After receiving the part list you have submitted, OSS verifies the validity of each data part individually. After all the data parts have been verified, OSS combines these parts into a complete object.

## Request syntax {#section_xfs_ftx_wdb .section}

```
POST /ObjectName? uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: Size
Authorization: Signature

<CompleteMultipartUpload>
<Part>
<PartNumber>PartNumber</PartNumber>
<ETag>ETag</ETag>
</Part>

</CompleteMultipartUpload>
```

## Request parameters {#section_x4m_3tx_wdb .section}

During the Complete Multipart Upload operation, you can use encoding-type to encode the Key in the returned result.

|Name|Type|Description|
|:---|:---|:----------|
|encoding-type|String|Specify the encoding type of the Key in the returned result. Currently, the URL encoding is supported. The Key adopts UTF-8 encoding, but the XML 1.0 Standard does not support parsing certain control characters, such as the characters with ASCII values from 0 to 10. In case that the Key contains control characters not supported by the XML 1.0 Standard, you can specify the encoding-type to encode the returned Key. Default: None

Optional value: url

|

## Request elements {#section_evg_qtx_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|CompleteMultipartUpload|Container|Container used for storing the content of the Complete Multipart Upload request.Sub-node: one or more part elements 

Parent node: None

|
|ETag|String|ETag value returned by OSS after data parts are successfully uploaded. Parent node: Part

|
|Part|Container|The container that saves uploaded data parts. Sub-nodes: ETag, PartNumber

Parent node: InitiateMultipartUploadResult

|
|PartNumber|Integer|Number of parts.Parent node: Part

|

## Response elements {#section_dps_15x_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|Bucket|String|Bucket name.Parent node: CompleteMultipartUploadResult

|
|CompleteMultipartUploadResult|Container|The container that stores the result of the Complete Multipart Upload request. Sub-nodes: Bucket, Key, ETag, Location Location

Parent node: None

|
|ETag|String|The ETag \(entity tag\) is created when an object is generated and is used to indicate the content of the object. For the objects created based on the Complete Multipart Upload request, the value of ETag is the UUID of the object content. The value of ETag can be used to check whether the content of the object is changed.Parent node: CompleteMultipartUploadResult

|
|Location|String|The URL of the newly created object. Parent node: CompleteMultipartUploadResult

|
|Key|String|Name of the newly created object. Parent node: CompleteMultipartUploadResult

|
|EncodingType|String|Specify the encoding type for the returned results. If encoding-type is specified in the request, the Key is encoded in the returned result.Parent node: Container

|

## Detail analysis {#section_byf_45x_wdb .section}

-   When receiving a Complete Multipart Upload request, OSS verifies that all parts except the last part are larger than 100 KB and checks each part number and ETag in the part list submitted by the user. Therefore, when uploading data parts, the client must record not only the part number but also the ETag value returned by OSS each time a part is uploaded successfully.
-   It takes a few minutes for OSS to process the Complete Multipart Upload request. During this time, if the client is disconnected from OSS, OSS continues to complete the request.
-   The part numbers in the part list submitted by a user can be non-consecutive. For example, the first part number is 1 and the second part number is 5.
-   After OSS successfully processes the Complete Multipart Upload request, the corresponding Upload ID becomes invalid.
-   The same object may have different Upload IDs. When an Upload ID is completed, other Upload IDs of this object are not affected.
-   If the x-oss-server-side-encryption request header is specified when the Initiate Multipart Upload interface is called, OSS returns the x-oss-server-side-encryption header in the Complete Multipart Upload response header.
-   The value of x-oss-server-side-encryption indicates the server-side encryption algorithm used for this object.

## Example {#section_ddv_t5x_wdb .section}

**Request example:**

```
POST /multipart.data? uploadId=0004B9B2D2F7815C432C9057C03134D4 HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length: 1056
Date: Fri, 24 Feb 2012 10:19:18 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:8VwFhFUWmVecK6jQlHlXMK/zMT0=

<CompleteMultipartUpload> 
    <Part> 
        <PartNumber>1</PartNumber>  
        <ETag>"3349DC700140D7F86A078484278075A9"</ETag> 
    </Part>  
    <Part> 
        <PartNumber>5</PartNumber>  
        <ETag>"8EFDA8BE206636A695359836FE0A0E0A"</ETag> 
    </Part>  
    <Part> 
        <PartNumber>8</PartNumber>  
        <ETag>"8C315065167132444177411FDA149B92"</ETag> 
    </Part> 
</CompleteMultipartUpload>
```

**Response example:**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Content-Length: 329
Content-Type: Application/xml
Connection: keep-alive
X-OSS-request-ID: 594f0751-3b1e-168f-4501-4ac71d217d6e
Date: Fri, 24 Feb 2012 10:19:18 GMT

<? xml version="1.0" encoding="UTF-8"? >
<CompleteMultipartUploadResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Location>http://oss-example.oss-cn-hangzhou.aliyuncs.com /multipart.data</Location>
    <Bucket>oss-example</Bucket>
    <Key>multipart.data</Key>
    <ETag>B864DB6A936D376F9F8D3ED3BBE540DD-3</ETag>
</CompleteMultipartUploadResult>
```

