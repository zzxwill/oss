# GetVodPlaylist {#concept_rqq_gw2_cgb .concept}

GetVodPlaylist接口用来查询指定 LiveChannel 推流生成的，指定时间段内的播放列表。

## 请求语法 {#section_b12_mw2_cgb .section}

```
GET /ChannelName?endTime=EndTime&startTime=StartTime HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## 请求参数 {#section_e5x_4w2_cgb .section}

|名称|描述|是否必需|
|ChannelName|指定 LiveChannel 名称，该LiveChannel 必须存在。|是|
|StartTime|指定查询 ts 文件的起始时间，格式为 Unix timestamp。|是|
|EndTime|指定查询 ts 文件的终止时间，格式为 Unix timestamp。**说明：** EndTime 必须大于 StartTime，且时间跨度不能大于 1 天。

|是|

## 示例 {#section_ewp_bxf_cgb .section}

请求示例

```
POST /test-channel?endTime=1472020226&startTime=1472020031 HTTP/1.1
Date: Thu, 25 Aug 2016 07:13:26 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:ABIigvnLtCHK+7fMHLeRlOUnzv0=
```

返回示例

```
HTTP/1.1 200
content-length: 312
server: AliyunOSS
connection: close
etag: "9C6104DD9CF1A0C4D0CFD21F43905D59"
x-oss-request-id: 57BE9A96B92475920B002359
date: Thu, 25 Aug 2016 07:13:26 GMT
Content-Type: application/x-mpegURL

#EXTM3U
#EXT-X-VERSION:3
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-TARGETDURATION:13
#EXTINF:7.120,
1543895706266.ts
#EXTINF:5.840,
1543895706323.ts
#EXTINF:6.400,
1543895706356.ts
#EXTINF:5.520,
1543895706389.ts
#EXTINF:5.240,
1543895706428.ts
#EXTINF:13.320,
1543895706468.ts
#EXTINF:5.960,
1543895706538.ts
#EXTINF:6.520,
1543895706561.ts
#EXT-X-ENDLIST
```

