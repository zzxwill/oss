# GetBucket \(List Object\) {#reference_iwr_xlv_tdb .reference}

The GetBucket operation can be used to list all of the object information in a bucket.

## Request syntax {#section_kmy_mdw_bz .section}

```
GET / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request parameters {#section_bqw_ndw_bz .section}

When you initiate a GetBucket \(ListObject\) request, you can use prefix, marker, delimiter, and max-keys to prescribe a limit to the list to return partial results. Besides, encoding-type can be used to encode the following elements in the returned results: delimiter, marker, prefix, NextMarker, and key.

|Name|Data type|Required|Description|
|----|---------|--------|-----------|
|delimiter|string|No|A character used to group object names. All the names of the objects that contain a specified prefix and after which the delimiter occurs for the first time, act as a group of elements - CommonPrefixes.Default value: None

|
|marker|string|No|Sets the returned results to begin from the first entry after the marker in alphabetical order.Default value: None

|
|max-keys|string|No|Limits the maximum number of objects returned for one request. If not specified, the default value is 100. The max-keys value cannot exceed 1000.Default value: 100

|
|prefix|string|No|Limits that the returned object  key must be prefixed accordingly. Note that the keys returned from queries using a prefix still contain the prefix.Default value: None

|
|encoding-type|string|No|Specifies the encoding of the returned content and the encoding type. Parameters delimiter, marker, prefix, NextMarker, and key use UTF-8 characters, but the XML  1.0 Standard does not support parsing certain control characters, such as characters with ASCII values ranging from 0 to 10. If some elements in the returned results contain characters that are not supported by the XML 1.0 Standard, encoding-type can be specified to encode these elements, such as delimiter, marker, prefix, NextMarker, and key.Default value: None;

Optional value: URL

|

## Response elements {#section_cfq_tdw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|Contents|container|Container used for saving every returned object meta.Parent node: ListBucketResult

|
|CommonPrefixes|string|If the delimiter parameter is specified in the request, the response returned by OSS contains the CommonPrefixes element. This element indicates the set of objects which ends with a delimiter and have a common prefix.Parent node: ListBucketResult

|
|Delimiter|string|A character used to group object names. All those objects whose names contain the specified prefix and after which the delimiter occurs for the first time, act as a group of elements - CommonPrefixes.Parent node: ListBucketResult

|
|EncodingType|string|Encoding type for the returned results. If encoding-type is specified in a request, the following elements in the returned results are encoded: delimiter, marker, prefix, NextMarker, and key.Parent node: ListBucketResult

|
|DisplayName|string|Name of the object owner.Parent node: ListBucketResult.Contents.Owner

|
|ETag|string|The ETag \(entity tag\) is created when an object is generated and is used to indicate the content of the object. For an object created by a Put  Object request, the value of ETag is the value of MD5 in the content of the object. For an object created in other way, the value of ETag is the UUID in the content of the object. The value of ETag can be used to check whether the content of the object is changed. We recommend that the ETag be used as the MD5 value of the object content to verify data integrity.Parent node: ListBucketResult.Contents

|
|ID|string|User ID of the bucket owner.Parent node: ListBucketResult.Contents.Owner

|
|IsTruncated|enumerated string|Indicates whether all results have been returned;  “true” means that not all results are returned this time; “false” means that all results are returned this time.Valid values: true and false

Parent node: ListBucketResult

|
|Key|string|Key of an objectParent node: ListBucketResult.Contents

|
|LastModified|time|The latest modification time of an object.Parent node: ListBucketResult.Contents

|
|ListBucketResult|container|Container for storing the results of the “Get Bucket” requestsubnodes: Name, Prefix, Marker, MaxKeys,  Delimiter, IsTruncated, Nextmarker, and  Contents

Parent node: None

|
|Marker|string|Marks the origin of the current Get Bucket \(List  Object\) request.Parent node: ListBucketResult

|
|MaxKeys|string|The maximum number of returned results in response to the request.Parent node: ListBucketResult

|
|Name|string|Name of a bucketParent node: ListBucketResult

|
|Owner|container|Container used for saving the information about the bucket owner.subnodes: DisplayName  and ID

Parent node: ListBucketResult

|
|Prefix|string|Starting prefix for the current results of query.Parent node: ListBucketResult

|
|Size|string|Number of bytes of the object.Parent node: ListBucketResult.Contents

|
|StorageClass|string|Indicates Object storage type. “Standard”, “IA”, and “Archive” types are available. \(Currently, the “Archive” type is only available in some regions.\)Parent node: ListBucketResult.Contents

|

## Detail analysis {#section_vnl_h2w_bz .section}

-   The custom meta in the object is not returned during the GetBucket request.
-   If the bucket to be accessed does not exist, or if you attempt to access a bucket which cannot be created because of standard naming rules are not followed when naming a bucket, Error 404 Not Found with the error code “NoSuchBucket” is returned.
-   If you have no permission to access the bucket, the system returns Error 403 Forbidden with the error code “AccessDenied”.
-   If listing cannot be completed at one time because of the max-keys setting, a `<NextMarker>` is appended to the returned result, prompting that this can be taken as a marker for continued listing. The value in NextMarker is still in the list result.
-   During a condition query, even if the marker does not exist in the list actually, what is returned is printed starting from the next to what conforms to the marker letter sorting. If the max-keys value is less than 0 or greater than 1000, error 400 Bad Request is returned. The error code is “InvalidArgument”.
-   If the prefix, marker, or delimiter parameters do not meet the length requirement, 400 Bad Request is returned. The error code is “InvalidArgument”.
-   The prefix and marker parameters are used to achieve display by pages, and the parameter length must be less than 1024 bytes.
-   Setting a prefix as the name of a folder lists the files starting with this prefix, recursively returning all files and subfolders in this folder. Additionally, if we set the Delimiter as “/“,  the returned values lists the files in the folder and the subfolders are returned in the CommonPrefixes section. Recursive files and folders in the subfolders are not displayed. For example, a bucket has the following three objects:  fun/test.jpg, fun/movie/001.avi, and fun/movie/007.avi. If the prefix is set to “fun/“,  three objects are returned. If the delimiter is set to “/“ additionally, file “fun/test.jpg” and prefix “fun/movie/“ are returned. That is, the folder logic is achieved.

## Scenario example {#section_b5f_j2w_bz .section}

Four objects are available in the bucket “my\_oss” and are named as:

-   oss.jpg
-   fun/test.jpg
-   fun/movie/001.avi
-   fun/movie/007.avi

## Example {#section_ujb_k2w_bz .section}

**Request example:**

```
GET / HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 08:43:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykboO4M=
```

**Return example:**

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

**Example of a request containing the prefix parameter:**

```
GET /? prefix=fun HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 08:43:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykboO4M=
```

**Return example:**

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

**Example of a request containing parameters prefix and delimiter:**

```
GET /? prefix=fun/&delimiter=/ HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 08:43:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY1vY=
```

**Return example:**

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

