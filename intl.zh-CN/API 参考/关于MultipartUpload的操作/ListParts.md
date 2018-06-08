# ListParts {#reference_hzm_1zx_wdb .reference}

ListParts接口用于列举指定Upload ID所属的所有已经上传成功Part。

## 请求语法 {#section_hsz_kzx_wdb .section}

```
Get  /ObjectName?uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: Signature
```

## 请求参数\(Request Parameters\) {#section_wsj_nzx_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|uploadId|字符串|Multipart Upload事件的ID。默认值：无

|
|max-parts|整数|规定在OSS响应中的最大Part数目。默认值：1,000

|
|part-number-marker|整数|指定List的起始位置，只有Part Number数目大于该参数的Part会被列出。默认值：无

 |
|encoding-type|字符串|指定对返回的内容进行编码，指定编码的类型。Key使用UTF-8字符，但xml 1.0标准不支持解析一些控制字符，比如ascii值从0到10的字符。对于Key中包含xml 1.0标准不支持的控制字符，可以通过指定encoding-type对返回的Key进行编码。默认值：无

可选值：url

|

## 响应元素\(Response Elements\) {#section_omr_mby_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|ListPartsResult|容器|保存List Part请求结果的容器。子节点：Bucket, Key, UploadId, PartNumberMarker, NextPartNumberMarker, MaxParts, IsTruncated, Part

父节点：无

|
|Bucket|字符串|Bucket名称。父节点：ListPartsResult

 |
|EncodingType|字符串|指明对返回结果进行编码使用的类型。如果请求的参数中指定了encoding-type，那会对返回结果中的Key进行编码。父节点：ListPartsResult

|
|Key|字符串|Object名称。父节点：ListPartsResult

|
|UploadId|字符串|Upload事件ID。父节点：ListPartsResult

 |
|PartNumberMarker|整数|本次List结果的Part Number起始位置。父节点：ListPartsResult

|
|NextPartNumberMarker|整数|如果本次没有返回全部结果，响应请求中将包含NextPartNumberMarker元素，用于标明接下来请求的PartNumberMarker值。父节点：ListPartsResult

 |
|MaxParts|整数|返回请求中最大的Part数目。父节点：ListPartsResult

|
|IsTruncated|枚举字符串|标明是否本次返回的List Part结果列表被截断。“true”表示本次没有返回全部结果；“false”表示本次已经返回了全部结果。有效值：true、false

父节点：ListPartsResult

|
|Part|字符串|保存Part信息的容器子节点：PartNumber,LastModified, ETag, Size

父节点：ListPartsResult

|
|PartNumber|整数|标示Part的数字。父节点：ListPartsResult.Part

|
|LastModified|日期|Part上传的时间。父节点：ListPartsResult.part

|
|ETag|字符串|已上传Part内容的ETag。 父节点：ListPartsResult.Part

 |
|Size|整数|已上传Part大小。父节点：ListPartsResult.Part

|

## 细节分析 {#section_bpx_qcy_wdb .section}

-   List Parts支持max-parts和part-number-marker两种请求参数。
-   max-parts参数最大值为1000；默认值也为1000。
-   在OSS的返回结果按照Part号码升序排列。
-   由于网络传输可能出错，所以不推荐用List Part出来的结果（Part Number和ETag值）来生成最后Complete Multipart的Part列表。

## 示例 {#section_gp1_5cy_wdb .section}

**请求示例：**

```
Get  /multipart.data?uploadId=0004B999EF5A239BB9138C6227D69F95  HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Thu, 23 Feb 2012 07:13:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:4qOnUMc9UQWqkz8wDqD3lIsa9P8=
```

**返回示例：**

```
HTTP/1.1 200 
Server: AliyunOSS
Connection: keep-alive
Content-length: 1221
Content-type: application/xml
x-oss-request-id: 106452c8-10ff-812d-736e-c865294afc1c
Date: Thu, 23 Feb 2012 07:13:28 GMT

<?xml version="1.0" encoding="UTF-8"?>
<ListPartsResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Bucket>multipart_upload</Bucket>
    <Key>multipart.data</Key>
    <UploadId>0004B999EF5A239BB9138C6227D69F95</UploadId>
    <NextPartNumberMarker>5</NextPartNumberMarker>
    <MaxParts>1000</MaxParts>
    <IsTruncated>false</IsTruncated>
    <Part>
        <PartNumber>1</PartNumber>
        <LastModified>2012-02-23T07:01:34.000Z</LastModified>
        <ETag>&quot;3349DC700140D7F86A078484278075A9&quot;</ETag>
        <Size>6291456</Size>
    </Part>
    <Part>
        <PartNumber>2</PartNumber>
        <LastModified>2012-02-23T07:01:12.000Z</LastModified>
        <ETag>&quot;3349DC700140D7F86A078484278075A9&quot;</ETag>
        <Size>6291456</Size>
    </Part>
    <Part>
        <PartNumber>5</PartNumber>
        <LastModified>2012-02-23T07:02:03.000Z</LastModified>
        <ETag>&quot;7265F4D211B56873A381D321F586E4A9&quot;</ETag>
        <Size>1024</Size>
    </Part>
</ListPartsResult>
```

