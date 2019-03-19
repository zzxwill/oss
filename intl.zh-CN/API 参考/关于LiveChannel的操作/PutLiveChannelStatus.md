# PutLiveChannelStatus {#reference_r5k_kcd_xdb .reference}

LiveChannel有两种Status：enabled和disabled，用户可以使用本接口在两种Status之间进行切换。

处于disabled状态时，OSS会禁止用户向该LiveChannel进行推流操作；如果有用户正在向该LiveChannel推流，那么推流的客户端会被强制断开（可能会有10s左右的延迟）。

## 请求语法 {#section_pjq_4cd_xdb .section}

PUT /ChannelName?live&status=NewStatus HTTP/1.1Date: GMT dateHost: BucketName.oss-cn-hangzhou.aliyuncs.comAuthorization: SignatureValue

## 请求参数 {#section_uqp_pcd_xdb .section}

|名称|描述|是否必需|
|:-|:-|:---|
|NewStatus|指定LiveChannel的目标Status。有效值：enabled、disabled

|是|

## 细节分析 {#section_vxd_scd_xdb .section}

-   当没有客户端向该LiveChannel推流时，调用PutLiveChannel重新创建LiveChannel也可以达到修改Status的目的。
-   当有客户端向该LiveChannel推流时，无法调用PutLiveChannel重新创建LiveChannel，只能通过本接口修改LiveChannel的状态为disabled。

## 示例 {#section_wvh_5cd_xdb .section}

**请求示例**

```
PUT /test-channel?live&status=disabled HTTP/1.1
Date: Thu, 25 Aug 2016 05:37:38 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:X/mBrSbkNoqM/JoAfRC0ytyQ5pY=
```

**返回示例**

```
HTTP/1.1 200
content-length: 0
server: AliyunOSS
connection: close
x-oss-request-id: 57BE8422B92475920B002030
date: Thu, 25 Aug 2016 05:37:39 GMT
```

