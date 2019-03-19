# GetLiveChannelStat {#reference_ov4_n3d_xdb .reference}

GetLiveChannelStat接口用来获取指定LiveChannel的推流状态信息。

## 请求语法 {#section_wx4_p3d_xdb .section}

```
GET /ChannelName?live&comp=stat HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## 响应元素 {#section_o5c_r3d_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|LiveChannelStat|容器|保存GetLiveChannelStat返回结果的容器。子节点：Status，ConnectedTime，Video，Audio

父节点：无

|
|Status|枚举字符串|LiveChannel当前的推流状态描述。子节点：无

父节点：LiveChannelStat

有效值：Disabled，Live，Idle

|
|ConnnectedTime|字符串|当Status为Live时，表示当前客户端开始推流的时间，使用ISO8601格式表示。子节点：无

父节点：LiveChannelStat

|
|RemoteAddr|字符串|当Status为Live时，表示当前推流客户端的ip地址。子节点：无

父节点：LiveChannelStat

|
|Video|容器|当Status为Live时，保存视频流信息的容器。子节点：Width，Heigth，FrameRate，Bandwidth，Codec

父节点：LiveChannelStat

|
|Width|字符串|表示当前视频流的画面宽度（单位：像素）。子节点：无

父节点：Video

|
|Heigth|字符串|表示当前视频流的画面高度（单位：像素）。子节点：无

父节点：Video

|
|FrameRate|字符串|表示当前视频流的帧率。子节点：无

父节点：Video

|
|Bandwidth|字符串|表示当前视频流的码率（单位：B/s\)。子节点：无

父节点：Video

|
|Codec|枚举字符串|表示当前视频流的编码格式。子节点：无

父节点：Video

|
|Audio|容器|当Status为Live时，保存音频流信息的容器。子节点：SampleRate，Bandwidth，Codec

父节点：LiveChannelStat

|
|SampleRate|字符串|表示当前音频流的采样率。子节点：无

父节点：Audio

|
|Bandwidth|字符串|表示当前音频流的码率（单位：B/s\)。子节点：无

父节点：Audio

|
|Codec|枚举字符串|表示当前音频流的编码格式。子节点：无

父节点：Audio

|

## 细节分析 {#section_cvg_rjd_xdb .section}

-   Video，Audio容器只有在Status为Live时才会返回，但Status为Live时不一定会返回Video，Audio容器，例如，客户端已经连接到LiveChannel，但尚未发送音视频数据时不会返回。
-   Bandwidth为音频流/视频流最近一段时间内的平均码率，LiveChannel刚切换到Live状态时，返回的Bandwidth值可能为0。

## 示例 {#section_wc3_tjd_xdb .section}

**请求示例I**

```
GET /test-channel?live&comp=stat HTTP/1.1
Date: Thu, 25 Aug 2016 06:22:01 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:fOzwkAgVTVSO1VKLPIInQ0JYyOA=
```

**返回示例I**

```
HTTP/1.1 200
content-length: 100
server: AliyunOSS
connection: close
x-oss-request-id: 57BE8E89B92475920B002164
date: Thu, 25 Aug 2016 06:22:01 GMT
content-type: application/xml
<?xml version="1.0" encoding="UTF-8"?>
<LiveChannelStat>
  <Status>Idle</Status>
</LiveChannelStat>
```

**请求示例II**

```
GET /test-channel?live&comp=stat HTTP/1.1
Date: Thu, 25 Aug 2016 06:25:26 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:WeC5joEaRzfSSS8xK0tlo7WTK1I=
```

**返回示例II**

```
HTTP/1.1 200
content-length: 469
server: AliyunOSS
connection: close
x-oss-request-id: 57BE8F56B92475920B002187
date: Thu, 25 Aug 2016 06:25:26 GMT
content-type: application/xml
<?xml version="1.0" encoding="UTF-8"?>
<LiveChannelStat>
  <Status>Live</Status>
  <ConnectedTime>2016-08-25T06:25:15.000Z</ConnectedTime>
  <RemoteAddr>10.1.2.3:47745</RemoteAddr>
  <Video>
    <Width>1280</Width>
    <Height>536</Height>
    <FrameRate>24</FrameRate>
    <Bandwidth>0</Bandwidth>
    <Codec>H264</Codec>
  </Video>
  <Audio>
    <Bandwidth>0</Bandwidth>
    <SampleRate>44100</SampleRate>
    <Codec>ADPCM</Codec>
  </Audio>
</LiveChannelStat>
```

