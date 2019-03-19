# PutLiveChannelStatus {#reference_r5k_kcd_xdb .reference}

A LiveChannel can be enabled or disabled. You can use PutLiveChannelStatus to switch the status of a LiveChannel.

If a LiveChannel is in the disabled status, you cannot push streams to the LiveChannel. If you are pushing a stream to a LiveChannel when the status of the LiveChannel is switched to disabled, your client is disconnected from the LiveChannel \(there may be a delay of 10 seconds\).

## Request syntax {#section_pjq_4cd_xdb .section}

PUT /ChannelName?live&status=NewStatus HTTP/1.1Date: GMT dateHost: BucketName.oss-cn-hangzhou.aliyuncs.comAuthorization: SignatureValue

## Request parameter {#section_uqp_pcd_xdb .section}

|Parameter|Description|Required|
|:--------|:----------|:-------|
|NewStatus|Specifies the status of the LiveChannel.Valid values: enabled and disabled

|Yes|

## Detail analysis {#section_vxd_scd_xdb .section}

-   If no client is pushing streams to a LivaChannel, you can switch the status of the LiveChannel by using PutLiveChannel, which creates a new LiveChannel.
-   If a stream is being pushed to a LiveChannel by other clients, you cannot use PutLiveChannel to create a new LiveChannel. You can switch the status of the LiveChannel to disabled only by using PutLiveChannelStatus.

## Examples {#section_wvh_5cd_xdb .section}

Request example

```
PUT /test-channel?live&status=disabled HTTP/1.1
Date: Thu, 25 Aug 2016 05:37:38 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:X/mBrSbkNoqM/JoAfRC0ytyQ5pY=
```

Response example

```
HTTP/1.1 200
content-length: 0
server: AliyunOSS
connection: close
x-oss-request-id: 57BE8422B92475920B002030
date: Thu, 25 Aug 2016 05:37:39 GMT
```

