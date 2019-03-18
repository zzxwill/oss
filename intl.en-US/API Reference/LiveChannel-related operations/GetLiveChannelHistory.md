# GetLiveChannelHistory {#reference_kjz_yld_xdb .reference}

Obtains the stream pushing record of a LiveChannel.

## Request syntax {#section_wtq_bmd_xdb .section}

```
GET /ChannelName?live&comp=history HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## Response element {#section_kln_dmd_xdb .section}

|Element|Type|Description|
|:------|:---|:----------|
|LiveChannelHistory|Container|\\Specifies the container that stores the response to the GetLiveChannelHistory request.Sub-node: LiveRecord

Parent node: None

|
|LiveRecord|Container|Specifies the container that stores a stream pushing record.Sub-node: StartTime, EndTime, and RemoteAddr

Parent node: LiveChannelHistory

|
|StartTime|String|Indicates the time when the client starts to push the stream. The value of this parameter is in ISO8601 format.Sub-node: None

Parent node: LiveRecord

|
|EndTime|String|Indicates the time when the client stops to push the stream. The value of this parameter is in ISO8601 format.Sub-node: None

Parent node: LiveRecord

|
|RemoteAddr|String|Indicates the IP address of the client that pushes the stream.Sub-node: None

Parent node: LiveRecord

|

## Detail analysis {#section_t2r_lmd_xdb .section}

A maximum of 10 records of the streams recently pushed to the specified LiveChannel is returned.

## Examples {#section_znp_mmd_xdb .section}

Request example

```
GET /test-channel?live&comp=history HTTP/1.1
Date: Thu, 25 Aug 2016 07:00:12 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:pqgDBP8JXTXAytBoXpvNoZfo68k=
```

Response example

```
HTTP/1.1 200
content-length: 1892
server: AliyunOSS
connection: close
x-oss-request-id: 57BE977CB92475920B0022FB
date: Thu, 25 Aug 2016 07:00:12 GMT
content-type: application/xml
<?xml version="1.0" encoding="UTF-8"?>
<LiveChannelHistory>
  <LiveRecord>
    <StartTime>2016-07-30T01:53:21.000Z</StartTime>
    <EndTime>2016-07-30T01:53:31.000Z</EndTime>
    <RemoteAddr>10.101.194.148:56861</RemoteAddr>
  </LiveRecord>
  <LiveRecord>
    <StartTime>2016-07-30T01:53:35.000Z</StartTime>
    <EndTime>2016-07-30T01:53:45.000Z</EndTime>
    <RemoteAddr>10.101.194.148:57126</RemoteAddr>
  </LiveRecord>
  <LiveRecord>
    <StartTime>2016-07-30T01:53:49.000Z</StartTime>
    <EndTime>2016-07-30T01:53:59.000Z</EndTime>
    <RemoteAddr>10.101.194.148:57577</RemoteAddr>
  </LiveRecord>
  <LiveRecord>
    <StartTime>2016-07-30T01:54:04.000Z</StartTime>
    <EndTime>2016-07-30T01:54:14.000Z</EndTime>
    <RemoteAddr>10.101.194.148:57632</RemoteAddr>
  </LiveRecord>
</LiveChannelHistory>
```

