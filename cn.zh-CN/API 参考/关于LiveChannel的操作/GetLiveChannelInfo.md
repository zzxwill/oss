# GetLiveChannelInfo {#reference_ekp_dkd_xdb .reference}

GetLiveChannelInfo接口用来获取指定LiveChannel的配置信息。

## 请求语法 {#section_qln_kkd_xdb .section}

```
GET /ChannelName?live HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## 响应元素 {#section_plc_mkd_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|LiveChannelConfiguration|容器|保存GetLiveChannelInfo返回结果的容器。子节点：Description、Status、Target

父节点：无

|
|Description|字符串|LiveChannel的描述信息。子节点：无

父节点：LiveChannelConfiguration

|
|Status|枚举字符串|LiveChannel的状态信息。子节点：无

父节点：LiveChannelConfiguration

有效值：enabled、disabled

|
|Target|容器|保存LiveChannel转储配置的容器。子节点：Type、FragDuration、FragCount、PlaylistName

父节点：LiveChannelConfiguration

|
|Type|枚举字符串|当Type为HLS时，指定推流时转储文件类型。子节点：无

父节点：Target

有效值：HLS

|
|FragDuration|字符串|当Type为HLS时，指定每个ts文件的时长（单位：秒）。子节点：无

父节点: Target

|
|FragCount|字符串|当Type为HLS时，指定m3u8文件中包含ts文件的个数。子节点：无

父节点：Target

|
|PlaylistName|字符串|当Type为HLS时，指定生成的m3u8文件的名称。子节点：无

父节点：Target

|

## 细节分析 {#section_kj3_rld_xdb .section}

Target的子节点FragDuration，FragCount，PlaylistName只有当Type取值为HLS时才会返回。

## 示例 {#section_q1g_sld_xdb .section}

**请求示例**

```
GET /test-channel?live HTTP/1.1
Date: Thu, 25 Aug 2016 05:52:40 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:D6bDCRXKht58hin1BL83wxyGvl0=
```

**返回示例**

```
HTTP/1.1 200
content-length: 475
server: AliyunOSS
connection: close
x-oss-request-id: 57BE87A8B92475920B002098
date: Thu, 25 Aug 2016 05:52:40 GMT
content-type: application/xml
<?xml version="1.0" encoding="UTF-8"?>
<LiveChannelConfiguration>
  <Description></Description>
  <Status>enabled</Status>
  <Target>
    <Type>HLS</Type>
    <FragDuration>2</FragDuration>
    <FragCount>3</FragCount>
    <PlaylistName>playlist.m3u8</PlaylistName>
  </Target>
</LiveChannelConfiguration>
```

