# ListLiveChannel {#reference_idb_smd_xdb .reference}

Lists specified LiveChannels.

## Request syntax {#section_kgc_dnd_xdb .section}

```
GET /?live HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## Request parameter {#section_iyf_syd_xdb .section}

|Parameter|Description|Required|
|:--------|:----------|:-------|
|marker|Indicates that the results after the marker are returned in alphabetical order.|No|
|max-keys|Specifies the maximum number of the returned LiveChannels.Default value: 100

Maximum value: 1000

|No|
|prefix|Specifies that only LiveChannels with the prefix are returned. When you use the prefix parameter to query LiveChannels, it is also included in the returned keys.|No|

## Response elements {#section_y1y_5yd_xdb .section}

|Element|Type|Description|
|:------|:---|:----------|
|ListLiveChannelResult|Container|Specifies the container that stores the response to the ListLiveChannel request.Sub-node: Prefix, Marker, MaxKeys, and IsTruncated, NextMarker, and LiveChannel

Parent node: None

|
|Prefix|String|Specifies the prefix of the query result.Sub-node: None

Parent node: ListLiveChannelResult

|
|Marker|String|Indicates that the LiveChannels after the marker in alphabetical order are returned.Sub-node: None

Parent node: ListLiveChannelResult

|
|MaxKeys|String|Specifies the maximum number of returned LiveChannels in the response.Sub-node: None

Parent node: ListLiveChannelResult

|
|IsTruncated|String|Indicates whether all results are returned. The value true indicates that not all results are returned, and value false indicates that all results are returned. Sub-node: None

Parent node: ListLiveChannelResult

|
|NextMarker|String|If not all results are returned, this element is included in the response to indicates the value of Marker for the next request.Sub-node: None

Parent node: ListLiveChannelResult

|
|LiveChannel|Container|Specifies the container that stores the information about a returned LiveChannel.Sub-node: Name, Description, Status, LastModified, PublishUrls, and PlayUrls

Parent node: ListLiveChannelResult

|
|Name|String|Indicates the name of the returned LiveChannel.Sub-node: None

Parent node: LiveChannel

|
|Description|String|Specifies the description of the returned LiveChannel.Sub-node: None

Parent node: LiveChannel

|
|Status|Enumerated string|Indicates the status of the returned LiveChannel.Sub-node: None

Parent node: LiveChannel

Valid value: disabled and enabled

|
|LastModified|String|Indicates the last modification time of the returned LiveChannel. The value of this parameter is in ISO8601 format.Sub-node: None

Parent node: LiveChannel

|
|PublishUrls|Container|Specifies the container that stores the URL used to push a stream to the LiveChannel.Sub-node: Url

Parent node: LiveChannel

|
|Url|String|Specifies the URL used to push a stream to the LiveChannel.Sub-node: None

Parent node: PublishUrls

|
|PlayUrls|Container|Specifies the container that stores the URL used to play a stream pushed to the LiveChannel.Sub-node: Url

Parent node: LiveChannel

|
|Url|String|Specifies the URL used to play the stream pushed to the LiveChannel. Sub-node: None

Parent node: PlayUrls

|

## Examples {#section_zcr_b12_xdb .section}

Request example

```
GET /?live&max-keys=1 HTTP/1.1
Date: Thu, 25 Aug 2016 07:50:09 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:TaX+tlc/Xsgpz6uRuqcbmUJsIHw=
```

Response example

```
HTTP/1.1 200
content-length: 656
server: AliyunOSS
connection: close
x-oss-request-id: 57BEA331B92475920B00245E
date: Thu, 25 Aug 2016 07:50:09 GMT
content-type: application/xml
<?xml version="1.0" encoding="UTF-8"?>
<ListLiveChannelResult>
  <Prefix></Prefix>
  <Marker></Marker>
  <MaxKeys>1</MaxKeys>
  <IsTruncated>true</IsTruncated>
  <NextMarker>channel-0</NextMarker>
  <LiveChannel>
    <Name>channel-0</Name>
    <Description></Description>
    <Status>disabled</Status>
    <LastModified>2016-07-30T01:54:21.000Z</LastModified>
    <PublishUrls>
      <Url>rtmp://test-bucket.oss-cn-hangzhou.aliyuncs.com/live/channel-0</Url>
    </PublishUrls>
    <PlayUrls>
      <Url>http://test-bucket.oss-cn-hangzhou.aliyuncs.com/channel-0/playlist.m3u8</Url>
    </PlayUrls>
  </LiveChannel>
```

