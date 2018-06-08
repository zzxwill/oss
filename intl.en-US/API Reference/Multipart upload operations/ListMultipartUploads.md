# ListMultipartUploads {#reference_hj2_3wx_wdb .reference}

The ListMultipartUploads interface can be used to list all Multipart Upload events in execution, that is, Multipart Upload events that have been initiated but not completed or aborted.

The listing result returned by OSS contains a maximum of 1000 Multipart Upload messages. If you want to specify the number of Multipart Upload messages in the listing result returned by OSS, you can add the max-uploads parameter to the request. In addition, the IsTruncated element in the listing result returned by OSS indicates whether other Multipart Upload messages are contained.

## Request syntax {#section_fgy_kwx_wdb .section}

```
Get /? uploads HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: Signature
```

## Request parameters {#section_jnq_mwx_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|delimiter|String|A character used to group object names. All those objects whose names contain the specified prefix and behind which the delimiter occurs for the first time act as a group of elements - CommonPrefixes.|
|max-uploads|String|Specify the maximum number of multipart upload tasks returned for one request. If this parameter is not specified, the default value 1,000 is used. The max-uploads value cannot exceed 1,000. |
|key-marker|String|Used together with the upload-id-marker parameter to specify the starting position of the returned result. -   If the upload-id-marker parameter is not set, the query result includes Multipart tasks in which the lexicographic orders of all object names are greater than the value of the key-marker parameter.
-   If the upload-id-marker parameter is set, the query result includes Multipart tasks in which the lexicographic orders of all object names are greater than the value of the key-marker parameter and all Multipart Upload tasks in which the object names are the same as the value of the key-marker parameter but the Upload IDs are greater than the value of the upload-id-marker parameter.

 |
|prefix|String|Limit that the returned object key must be prefixed accordingly. Note that the keys returned from queries using a prefix still contain the prefix.|
|upload-id-marker|String|Used together with the key-marker parameter to specify the starting position of the returned result. -   If the key-marker parameter is not set, OSS ignores the upload-id-marker parameter.
-   If the key-marker parameter is set, the query result includes Multipart tasks in which the lexicographic orders of all object names are greater than the value of the key-marker parameter and all Multipart Upload tasks in which the object names are the same as the value of the key-marker parameter but the Upload IDs are greater than the value of the upload-id-marker parameter.

|
|encoding-type|String|Specify the encoding of the returned content and the encoding type. Delimiter, KeyMarker, Prefix, NextKeyMarker, and Key use UTF-8 characters, but the XML 1.0 Standard does not support parsing certain control characters, such as characters with ASCII values ranging from 0 to 10. If some elements in the returned results contain control characters that are not supported by the XML 1.0 Standard, encoding-type can be specified to encode these elements, such as Delimiter, KeyMarker, Prefix, NextMarker, and Key.Default: None

|

## Response elements {#section_zgj_wwx_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|ListMultipartUploadsResult|Container|The container that saves the result of the List Multipart Upload request. Child Nodes: Bucket, KeyMarker, UploadIdMarker, NextKeyMarker, NextUploadIdMarker, MasUploads, Delimiter, Prefix, CommonPrefixes, IsTruncated, Upload

Parent node: None

|
|Bucket|String|Bucket name.Parent node: ListMultipartUploadsResult

|
|EncodingType|String|Specify the encoding type for the returned results. If encoding-type is specified in the request, those elements including Delimiter, KeyMarker, Prefix, NextKeyMarker, and Key are encoded in the returned result.Parent node: ListMultipartUploadsResult

|
|KeyMarker|String|Position of the starting object in the list. Parent node: ListMultipartUploadsResult

|
|UploadIdMarker|String|Position of the starting Upload ID in the list.Parent node: ListMultipartUploadsResult

|
|NextKeyMarker|String|If not all results are returned this time, the response request includes the NextKeyMarker element to indicate the value of KeyMarker in the next request.Parent node: ListMultipartUploadsResult

|
|NextUploadMarker|String|If not all results are returned this time, the response request includes the NextUploadMarker element to indicate the value of UploadMarker in the next request.Parent node: ListMultipartUploadsResult

|
|MaxUploads|Integer|The maximum upload number returned by the OSS.Parent node: ListMultipartUploadsResult

|
|IsTruncated|enumerative string|Specify whether the returned Multipart Upload result list is truncated. The “true” indicates that not all results are returned; “false” indicates that all results are returned.Valid values: false, true 

Default: false 

Parent node: ListMultipartUploadsResult

|
|Upload|Container|The container that saves the information about the Multipart Upload event.Sub-nodes: Key, UploadId, Initiated

Parent node:ListMultipartUploadsResult

|
|Key|String|Name of an object for which a Multipart Upload event is initiated.Parent node: Upload

|
|UploadId|String|ID of a Multipart Upload event.Parent node: Upload

|
|Initiated|Date|Time when a Multipart Upload event is initiated.Parent node: Upload

|

## Detail analysis {#section_ilz_cyx_wdb .section}

-   The maximum value of the max-uploads parameter is 1,000.
-   The results returned by OSS are listed in ascending order based on the lexicographic orders of object names; for the same object, the results are listed in ascending time order.
-   Using the prefix parameter, you can flexibly manage objects in a bucket in groups \(similar to the folder function\).
-   The List Multipart Uploads request supports five request parameters: prefix, marker, delimiter, upload-id-marker, and max-uploads. Based on the combinations of these parameters, you can set rules for querying Multipart Uploads events to obtain the expected query results.

## Example {#section_m24_hyx_wdb .section}

**Request example:**

```
Get /? uploads HTTP/1.1
HOST: OSS-example.
Date: Thu, 23 Feb 2012 06:14:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:JX75CtQqsmBBz+dcivn7kwBMvOY=
```

**Response example:**

```
HTTP/1.1 200 
Server: AliyunOSS
Connection: keep-alive
Content-length: 1839
Content-type: application/xml
x-oss-request-id: 58a41847-3d93-1905-20db-ba6f561ce67a
Date: Thu, 23 Feb 2012 06:14:27 GMT

<? xml version="1.0" encoding="UTF-8"? >
<ListMultipartUploadsResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Bucket>oss-example</Bucket>
    <KeyMarker></KeyMarker>
    <UploadIdMarker></UploadIdMarker>
    <NextKeyMarker>oss.avi</NextKeyMarker>
    <NextUploadIdMarker>0004B99B8E707874FC2D692FA5D77D3F</NextUploadIdMarker>
    <Delimiter></Delimiter>
    <Prefix></Prefix>
    <MaxUploads>1000</MaxUploads>
    <IsTruncated>false</IsTruncated>
    <Upload>
        <Key>multipart.data</Key>
        <UploadId>0004B999EF518A1FE585B0C9360DC4C8</UploadId>
        <Initiated>2012-02-23T04:18:23.000Z</Initiated>
    </Upload>
    <Upload>
        <Key>multipart.data</Key>
        <UploadId>0004B999EF5A239BB9138C6227D69F95</UploadId>
        <Initiated>2012-02-23T04:18:23.000Z</Initiated>
    </Upload>
    <Upload>
        <Key>oss.avi</Key>
        <UploadId>0004B99B8E707874FC2D692FA5D77D3F</UploadId>
        <Initiated>2012-02-23T06:14:27.000Z</Initiated>
    </Upload>
</ListMultipartUploadsResult>
```

