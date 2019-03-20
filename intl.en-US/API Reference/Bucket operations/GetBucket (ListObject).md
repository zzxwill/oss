# GetBucket \(ListObject\) {#reference_iwr_xlv_tdb .reference}

Lists the information about all objects in a bucket.

## Request syntax {#section_kmy_mdw_bz .section}

```
GET / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request elements {#section_bqw_ndw_bz .section}

When you initiate a GetBucket \(ListObject\) request, you can use prefix, marker, delimiter, and max-keys to prescribe a limit to the ListObject operation to return partial results.

|Element|Type|Required|Description|
|-------|----|--------|-----------|
|delimiter|String|No|Specifies a character used to group object names. All the names of the objects that contain a specified prefix and after which the delimiter occurs for the first time, act as a group of elements, that is, CommonPrefixes.Default value: None

|
|marker|String|No|Sets the returned results to begin from the first entry after the marker in alphabetical order.Default value: None

|
|max-keys|String|No|Limits the maximum number of objects returned for one request. The max-keys value cannot exceed 1000.Default value: 100

If the listing operation cannot be completed at one time because of the limits set by max-keys. A `<NextMarker>` is included in the response to indicates the marker for the next listing operation.

|
|prefix|String|No|Limits that the returned object  key must be prefixed accordingly. Note that the keys returned from queries using a prefix still contain the prefix.Default value: None

|
|encoding-type|String|No|Encodes the returned results and specifies the encoding type. Parameters delimiter, marker, prefix, NextMarker, and key use UTF-8 characters, but the XML  1.0 Standard does not support parsing certain control characters, such as characters with ASCII values ranging from 0 to 10. If some elements in the returned results contain characters that are not supported by the XML 1.0 Standard, encoding-type can be specified to encode these elements, such as delimiter, marker, prefix, NextMarker, and key.Default value: None

Optional value: url

**Note:** XML 1.0 does not support parsing certain control characters, such as characters with ASCII values ranging from 0 to 10. If some elements in the returned results contain characters that are not supported by XML 1.0, you can set the value of encoding-type to encode these elements, such as delimiter, marker, prefix, NextMarker, and key.

|

## Response elements {#section_cfq_tdw_bz .section}

|Element|Type|Description|
|-------|----|-----------|
|Contents|Container|Indicates the container used to store every returned object meta.Parent node: ListBucketResult

|
|CommonPrefixes|String|If the delimiter parameter is specified in the request, the response returned by OSS contains the CommonPrefixes element. This element indicates the set of objects which ends with a delimiter and have a common prefix.Parent node: ListBucketResult

|
|Delimiter|String|Indicates a character used to group object names. All those objects whose names contain the specified prefix and after which the delimiter occurs for the first time, act as a group of elements, that is, CommonPrefixes.Parent node: ListBucketResult

|
|EncodingType|String|Indicates the encoding type for the returned results. If encoding-type is specified in a request, the following elements in the returned results are encoded: delimiter, marker, prefix, NextMarker, and key.Parent node: ListBucketResult

|
|DisplayName|String|Indicates the name of the object owner.Parent node: ListBucketResult.Contents.Owner

|
|ETag|String|The ETag \(entity tag\) is created when an object is generated and is used to indicate the content of the object.Parent node: ListBucketResult.Contents

For an object created by a PutObject request, the value of ETag is the value of MD5 in the content of the object. For an object created in other way, the value of ETag is the UUID in the content of the object. The value of ETag can be used to check whether the content of the object is changed. We recommend that the ETag be used as the MD5 value of the object content to verify data integrity.

|
|ID|String|User ID of the bucket owner.Parent node: ListBucketResult.Contents.Owner

|
|IsTruncated|Enumerated string|Indicates whether all results are returned.Valid values: true and false

-   true indicates that not all results are returned for the request.
-   false indicates that all results are returned for the request.

Parent node: ListBucketResult

|
|Key|String|Indicates the key of an objectParent node: ListBucketResult.Contents

|
|LastModified|Time|Indicates the time when the object is last modified.Parent node: ListBucketResult.Contents

|
|ListBucketResult|Container|Indicates the container used to store the results of the GetBucket \(ListObject\) request.Sub-node: Name, Prefix, Marker, MaxKeys,  Delimiter, IsTruncated, Nextmarker, and  Contents

Parent node: None

|
|Marker|String|Marks the position where the current GetBucket \(ListObject\) operation starts.Parent node: ListBucketResult

|
|MaxKeys|String|Indicates the maximum number of returned results in the response to the request.Parent node: ListBucketResult

|
|Name|String|Indicates the name of the bucket.Parent node: ListBucketResult

|
|Owner|Container|Indicates the container used to store the information about the bucket owner.Sub-node: DisplayName and ID

Parent node: ListBucketResult

|
|Prefix|String|Indicates the prefix of results returned for the request.Parent node: ListBucketResult

|
|Size|String|Indicates the number of bytes of the object.Parent node: ListBucketResult.Contents

|
|StorageClass|String|Indicates the storage class of an object. Only the Standard storage class is supported.Parent node: ListBucketResult.Contents

|

## Detail analysis {#section_vnl_h2w_bz .section}

-   The custom meta in the object is not returned during the GetBucket request.
-   If the bucket to be accessed does not exist, a 404 Not Found error is returned with the error code NoSuchBucket.
-   If you have no permission to access the bucket, OSS returns a 403 Forbidden error with the error code AccessDenied.
-   During a conditional query, even if the marker does not exist in the list, the results are printed starting from the letter next to marker in alphabetical order. If the value of max-keys is less than 0 or greater than 1000, a 400 Bad Request error is returned with the error code InvalidArgument.
-   If the length of the Prefix, Marker, and Delimiter parameters does not meet the requirement, a 400 Bad Request error is returned with the error code InvalidArgument.
-   The Prefix and Marker parameters are used to display the results by pages, and the parameter length must be less than 1024 bytes.
-   If you set the value of Prefix to a directory name, you can list all objects with the prefix, that is, all objects and sub-directories in the directory.

    If you set the Prefix and set Delimiter to “/“, only the objects in the directory are returned. Sub-directories in the directory are returned in CommonPrefixes. All objects and directories in the sub-directories are not displayed.

    For example, the following three objects are stored in a bucket: fun/test.jpg, fun/movie/001.avi, and fun/movie/007.avi. If the Prefix is set to “fun/“,  all three objects are returned. If the delimiter is set to “/“ additionally, “fun/test.jpg” and “fun/movie/“ are returned.


## Examples {#section_ujb_k2w_bz .section}

Simple request example:

```
GET / HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 08:43:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykboO4M=
```

Response example:

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 08:43:27 GMT
Content-Type: application/xml
Content-Length: 1866
Connection: keep-alive
Server: AliyunOSS
<? xml version="1.0" encoding="UTF-8"? >
<ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
<Name>oss-example</Name>
<Prefix></Prefix>
<Marker></Marker>
<MaxKeys>100</MaxKeys>
<Delimiter></Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>fun/movie/001.avi</Key>
        <LastModified>2012-02-24T08:43:07.000Z</LastModified>
        <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
        <Type>Normal</Type>
        <Size>344606</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
            <ID>00220120222</ID>
            <DisplayName>user-example</DisplayName>
        </Owner>
    </Contents>
    <Contents>
        <Key>fun/movie/007.avi</Key>
        <LastModified>2012-02-24T08:43:27.000Z</LastModified>
        <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
        <Type>Normal</Type>
        <Size>344606</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
            <ID>00220120222</ID>
            <DisplayName>user-example</DisplayName>
        </Owner>
    </Contents>
<Contents>
        <Key>fun/test.jpg</Key>
        <LastModified>2012-02-24T08:42:32.000Z</LastModified>
        <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
        <Type>Normal</Type>
        <Size>344606</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
            <ID>00220120222</ID>
            <DisplayName>user-example</DisplayName>
        </Owner>
    </Contents>
    <Contents>
        <Key>oss.jpg</Key>
        <LastModified>2012-02-24T06:07:48.000Z</LastModified>
        <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
        <Type>Normal</Type>
        <Size>344606</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
            <ID>00220120222</ID>
            <DisplayName>user-example</DisplayName>
        </Owner>
    </Contents>
</ListBucketResult>
```

Example of a request including the prefix parameter:

```
GET /? prefix=fun HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 08:43:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykboO4M=
```

Response example:

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 08:43:27 GMT
Content-Type: application/xml
Content-Length: 1464
Connection: keep-alive
Server: AliyunOSS
<? xml version="1.0" encoding="UTF-8"? >
<ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
<Name>oss-example</Name>
<Prefix>fun</Prefix>
<Marker></Marker>
<MaxKeys>100</MaxKeys>
<Delimiter></Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>fun/movie/001.avi</Key>
        <LastModified>2012-02-24T08:43:07.000Z</LastModified>
        <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
        <Type>Normal</Type>
        <Size>344606</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
            <ID>00220120222</ID>
            <DisplayName>user_example</DisplayName>
        </Owner>
    </Contents>
    <Contents>
        <Key>fun/movie/007.avi</Key>
        <LastModified>2012-02-24T08:43:27.000Z</LastModified>
        <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
        <Type>Normal</Type>
        <Size>344606</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
            <ID>00220120222</ID>
            <DisplayName>user_example</DisplayName>
        </Owner>
    </Contents>
    <Contents>
        <Key>fun/test.jpg</Key>
        <LastModified>2012-02-24T08:42:32.000Z</LastModified>
        <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
        <Type>Normal</Type>
        <Size>344606</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
            <ID>00220120222</ID>
            <DisplayName>user_example</DisplayName>
        </Owner>
    </Contents>
</ListBucketResult>
```

Example of a request including the prefix and delimiter parameters:

```
GET /? prefix=fun/&delimiter=/ HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 08:43:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY1vY=
```

Response example:

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 08:43:27 GMT
Content-Type: application/xml
Content-Length: 712
Connection: keep-alive
Server: AliyunOSS
<? xml version="1.0" encoding="UTF-8"? >
<ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
<Name>oss-example</Name>
<Prefix>fun/</Prefix>
<Marker></Marker>
<MaxKeys>100</MaxKeys>
<Delimiter>/</Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>fun/test.jpg</Key>
        <LastModified>2012-02-24T08:42:32.000Z</LastModified>
        <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
        <Type>Normal</Type>
        <Size>344606</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
            <ID>00220120222</ID>
            <DisplayName>user_example</DisplayName>
        </Owner>
    </Contents>
   <CommonPrefixes>
        <Prefix>fun/movie/</Prefix>
   </CommonPrefixes>
</ListBucketResult>
```

