# ListLiveChannel {#reference_idb_smd_xdb .reference}

ListLiveChannel接口用于罗列指定的LiveChannel。

## 请求语法 {#section_kgc_dnd_xdb .section}

```
GET /?live HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## 请求参数 {#section_iyf_syd_xdb .section}

|名称|描述|是否必需|
|:-|:-|:---|
|marker|设定结果从marker之后按字母排序的第一个开始返回。|否|
|max-keys|限定此次返回LiveChannel的最大数，如果不设定，默认为100，max-keys取值不能大于1000。默认值：100

|否|
|prefix|限定返回的LiveChannel必须以prefix作为前缀。注意使用prefix查询时，返回的key中仍会包含prefix。|否|

## 响应元素 {#section_y1y_5yd_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|ListLiveChannelResult|容器|保存ListLiveChannel请求结果的容器。子节点：Prefix，Marker，MaxKeys，IsTruncated，NextMarker，LiveChannel

父节点：无

|
|Prefix|字符串|本次查询结果的开始前缀。子节点：无

父节点：ListLiveChannelResult

|
|Marker|字符串|本次ListLiveChannel的起点。子节点：无

父节点：ListLiveChannelResult

|
|MaxKeys|字符串|响应请求内返回结果的最大数目。子节点：无

父节点：ListLiveChannelResult

|
|IsTruncated|字符串|指明是否所有的结果都已经返回； “true”表示本次没有返回全部结果；“false”表示本次已经返回了全部结果。子节点：无

父节点：ListLiveChannelResult

|
|NextMarker|字符串|如果本次没有返回全部结果，响应请求中将包含NextMarker元素，用于标明接下来请求的Marker值。子节点：无

父节点：ListLiveChannelResult

|
|LiveChannel|容器|保存返回每个LiveChannel信息的容器。子节点：Name，Description，Status，LastModified，PublishUrls，PlayUrls

父节点：ListLiveChannelResult

|
|Name|字符串|LiveChannel的名称。子节点：无

父节点：LiveChannel

|
|Description|字符串|LiveChannel的描述。子节点：无

父节点：LiveChannel

|
|Status|枚举字符串|LiveChannel的状态。子节点：无

父节点：LiveChannel

有效值：disabled，enabled

|
|LastModified|字符串|LiveChannel配置的最后修改时间，使用ISO8601格式表示。子节点：无

父节点：LiveChannel

|
|PublishUrls|容器|保存LiveChannel对应的推流地址的容器。子节点：Url

父节点：LiveChannel

|
|Url|字符串|LiveChannel对应的推流地址。子节点：无

父节点：PublishUrls

|
|PlayUrls|容器|保存LiveChannel对应的播放地址的容器。子节点：Url

父节点：LiveChannel

|
|Url|字符串|LiveChannel对应的播放地址。 子节点：无

父节点：PlayUrls

|

## 示例 {#section_zcr_b12_xdb .section}

**请求示例**

```
GET /?live&max-keys=1 HTTP/1.1
Date: Thu, 25 Aug 2016 07:50:09 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:TaX+tlc/Xsgpz6uRuqcbmUJsIHw=
```

**返回示例**

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

