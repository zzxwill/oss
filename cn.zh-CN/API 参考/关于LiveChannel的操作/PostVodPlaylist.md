# PostVodPlaylist {#reference_ry3_fhd_xdb .reference}

PostVodPlaylist接口用来为指定LiveChannel推流生成的，指定时间段内的ts文件生成一个点播用的播放列表（m3u8文件）。

## 请求语法 {#section_o5f_3hd_xdb .section}

```
POST /ChannelName/PlaylistName?vod&endTime=EndTime&startTime=StartTime HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## 请求参数 {#section_nmr_jhd_xdb .section}

|名称|描述|是否必需|
|:-|:-|:---|
|ChannelName|指定LiveChannel名称，该LiveChannel必须存在。|是|
|PlaylistName|指定生成的点播播放列表的名称，必须以“.m3u8”结尾。|是|
|StartTime|指定查询ts文件的起始时间，格式为Unix timestamp。|是|
|EndTime|指定查询ts文件的终止时间，格式为Unix timestamp。|是|

## 细节分析 {#section_hc5_lhd_xdb .section}

-   EndTime必须大于StartTime，且时间跨度不能大于1天。
-   OSS会查询指定时间范围内的所有该LiveChannel推流生成的ts文件，并将其拼装为一个播放列表。

## 示例 {#section_kqs_whd_xdb .section}

**请求示例**

```
POST /test-channel/vod.m3u8?vod&endTime=1472020226&startTime=1472020031 HTTP/1.1
Date: Thu, 25 Aug 2016 07:13:26 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:ABIigvnLtCHK+7fMHLeRlOUnzv0=
```

**返回示例**

```
HTTP/1.1 200
content-length: 0
server: AliyunOSS
connection: close
etag: "9C6104DD9CF1A0C4D0CFD21F43905D59"
x-oss-request-id: 57BE9A96B92475920B002359
date: Thu, 25 Aug 2016 07:13:26 GMT
```

