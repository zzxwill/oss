# GetObjectMeta {#reference_sg4_k2w_wdb .reference}

GetObjectMeta用来获取某个Bucket下的某个Object的基本meta信息，包括该Object的ETag、Size（文件大小）、LastModified，并不返回其内容。

## 请求语法 {#section_nvn_42w_wdb .section}

```
GET /ObjectName?objectMeta HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_b3b_q2w_wdb .section}

-   无论正常返回还是非正常返回，Get Object Meta均不返回消息体。
-   Get Object Meta需包含objectMeta请求参数，否则表示Get Object请求。
-   如果文件不存在返回404 Not Found错误。
-   Get Object Meta相比Head Object更轻量，仅返回指定Object的少量基本meta信息，包括该Object的ETag、Size（文件大小）、LastModified，其中Size由响应头Content-Length的数值表示。
-   如果文件类型为符号链接，返回符号链接自身信息。

## 示例 {#section_wkh_52w_wdb .section}

**请求示例：**

```
GET /oss.jpg?objectMeta HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Apr 2015 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0FmgbrQ0=
```

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Apr 2015 05:21:12 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE"
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
Content-Length: 344606
Connection: keep-alive
Server: AliyunOSS
```

