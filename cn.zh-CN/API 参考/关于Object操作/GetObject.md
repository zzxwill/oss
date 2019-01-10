# GetObject {#reference_ccf_rgd_5db .reference}

GetObject用于获取某个Object ，此操作要求用户对该Object有读权限。

## 请求语法 {#section_pjt_dmw_bz .section}

```
GET /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
Range: bytes=ByteRange(可选)
```

## 请求参数 {#section_gsg_gmw_bz .section}

OSS支持用户在发送GET请求时，可以自定义OSS返回请求中的一些Header。

**说明：** 用户发送的GET请求必须携带签名。

自定义返回请求的Header包括 ：

|名称|类型|描述|
|--|--|--|
|response-content-type|字符串|设置OSS返回请求的content-type头 默认值：无

|
|response-content-language|字符串|设置OSS返回请求的content-language头 默认值：无

|
|response-expires|字符串|设置OSS返回请求的expires头 默认值：无

|
|response-cache-control|字符串|设置OSS返回请求的cache-control头 默认值：无

|
|response-content-disposition|字符串|设置OSS返回请求的content-disposition头 默认值：无

|
|response-content-encoding|字符串|设置OSS返回请求的content-encoding头 默认值：无

|

**说明：** 在自定义OSS返回请求中的一些Header，只有请求处理成功（即返回码为 200 OK ），OSS才会将请求的Header设置成用户GET请求参数中指定的值。

OSS不支持在匿名访问的GET请求中，通过请求参数来自定义返回请求的Header。

## 请求Header {#section_ywk_qmw_bz .section}

|名称|类型|描述|
|--|--|--|
|Range|字符串| 指定文件传输的范围。

 默认值：无

 在请求Header中使用Range参数

-   符合范围规范，返回消息中则会包含整个Object的长度和此次返回的范围。例如：Content-Range: bytes 0-9/44，表示整个Object长度为44，此次返回的范围为0-9。
-   不符合范围规范，则传送整个Object，并且不在结果中提及Content-Range。

 |
|If-Modified-Since|字符串|如果指定的时间早于实际修改时间或指定的时间不符合规范，直接返回Object，并返回200 OK；否则返回304 not modified。默认值：无

时间格式：GMT时间，例如Fri, 13 Nov 2015 14:47:53 GMT

|
|If-Unmodified-Since|字符串|如果传入参数中的时间等于或者晚于Object实际修改时间，则正常传输Object，并返回200 OK；否则返回412 precondition failed错误 。默认值：无

时间格式：GMT时间，例如Fri, 13 Nov 2015 14:47:53 GMT

If-Modified-Since和If-Unmodified-Since可以同时使用。

|
|If-Match|字符串|如果传入期望的ETag和Object的ETag匹配，则正常传输Object，并返回200 OK；否则返回412 precondition failed错误。默认值：无

|
|If-None-Match|字符串|如果传入的ETag值和Object的ETag不匹配，则正常传输Object，并返回200 OK；否则返回304 Not Modified。默认值：无

If-Match和If-None-Match可以同时使用。

|

## 常见错误码： {#section_hnc_tz5_jgb .section}

|错误码|HTTP状态码|2说明|
|---|-------|---|
|NoSuchKey|404|Object不存在。|
|SymlinkTargetNotExist|404|Object类型为符号链接，并且目标Object不存在。|
|InvalidTargetType|400|Object类型为符号链接，并且目标Object类型为符号链接。|
|InvalidObjectState|403|对于归档类型（Archive）的Object，没有提交Restore请求或者上一次提交Restore已经超时。|
|InvalidObjectState|403|对于归档类型的Object，已经提交Restore请求，但数据的Restore操作还没有完成。|

## 细节分析 {#section_xb4_wmw_bz .section}

-   若Object为服务器端熵编码加密存储，则在GET请求时会自动解密返回给用户，并且在响应Header中返回x-oss-server-side-encryption，其值表明该Object的服务器端加密算法。
-   如果返回内容进行GZIP压缩传输，需要在请求的Header中以显示方式加入Accept-Encoding:gzip，OSS会根据Object的Content-Type和Object大小（不小于1KB），判断是否返回经过GZIP 压缩的数据。如果采用了GZIP压缩则不会附带ETag 信息。目前OSS支持GZIP压缩的Content-Type为HTML、Javascript、CSS、XML、RSS、Json。
-   如果Object类型为符号链接，返回目标Object的内容。响应头中`Content-Length`、`ETag`、`Content-Md5` 均为目标Object的元信息；`Last-Modified`是目标Object和符号链接的最大值；其他均为符号链接的元信息。
-   如果Object类型为归档类型，需要完成Restore请求且该请求不能超时。

## 示例 {#section_vrp_zmw_bz .section}

**简单的GET请求示例：**

```
GET /oss.jpg HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 06:38:30 GMT
Authorization:OSS qn6qrrqxo2oawuk53otfjbyc:UNQDb7GapEgJCZkcde6OhZ9Jfe8=
```

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 3a89276f-2e2d-7965-3ff9-51c875b99c41
x-oss-object-type: Normal
Date: Fri, 24 Feb 2012 06:38:30 GMT
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE "
Content-Type: image/jpg
Content-Length: 344606
Server: AliyunOSS
[344606 bytes of object data]
```

**带有Range参数的请求示例：**

```
GET //oss.jpg HTTP/1.1
Host:oss-example. oss-cn-hangzhou.aliyuncs.com
Date: Fri, 28 Feb 2012 05:38:42 GMT
Range: bytes=100-900
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:qZzjF3DUtd+yK16BdhGtFcCVknM=
```

**返回示例：**

```
HTTP/1.1 206 Partial Content
x-oss-request-id: 28f6508f-15ea-8224-234e-c0ce40734b89
x-oss-object-type: Normal
Date: Fri, 28 Feb 2012 05:38:42 GMT
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE "
Accept-Ranges: bytes
Content-Range: bytes 100-900/344606
Content-Type: image/jpg
Content-Length: 801
Server: AliyunOSS
[801 bytes of object data]
```

**带自定义返回消息头的请求示例：**

```
GET /oss.jpg?response-expires=Thu%2C%2001%20Feb%202012%2017%3A00%3A00%20GMT& response-content-type=text&response-cache-control=No-cache&response-content-disposition=attachment%253B%2520filename%253Dtesting.txt&response-content-encoding=utf-8&response-content-language=%E4%B8%AD%E6%96%87 HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com:
Date: Fri, 24 Feb 2012 06:09:48 GMT
```

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
x-oss-object-type: Normal
Date: Fri, 24 Feb 2012 06:09:48 GMT 
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE "
Content-Length: 344606
Connection: keep-alive
Content-disposition: attachment; filename:testing.txt
Content-language: 中文
Content-encoding: utf-8
Content-type: text
Cache-control: no-cache
Expires: Fri, 24 Feb 2012 17:00:00 GMT
Server: AliyunOSS
[344606 bytes of object data]
```

**Object类型为符号链接的请求示例：**

```
GET /link-to-oss.jpg HTTP/1.1
Accept-Encoding: identity
Date: Tue, 08 Nov 2016 03:17:58 GMT
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Authorization:OSS qn6qrrqxo2oawuk53otfjbyc:qZzjF3DUtd+yK16BdhGtFcCVknM=
```

**返回示例：**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Tue, 08 Nov 2016 03:17:58 GMT
Content-Type: application/octet-stream
Content-Length: 20
Connection: keep-alive
x-oss-request-id: 582143E6D3436A212ADCC87D
Accept-Ranges: bytes
ETag: "8086265EFC0211ED1F9A2F09BF462227"
Last-Modified: Tue, 08 Nov 2016 03:17:58 GMT
x-oss-object-type: Symlink
Content-MD5: gIYmXvwCEe0fmi8Jv0YiJw==
```

**Restore操作已经完成的请求示例：**

```
GET /oss.jpg HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 09:38:30 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:zUglwRPGkbByZxm1+y4eyu+NIUs=
```

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 58F723894529F18D7F000053
x-oss-object-type: Normal
x-oss-restore: ongoing-request="false", expiry-date="Sun, 16 Apr 2017 08:12:33 GMT"
Date: Sat, 15 Apr 2017 09:38:30 GMT
Last-Modified: Sat, 15 Apr 2017 06:07:48 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE "
Content-Type: image/jpg
Content-Length: 344606
Server: AliyunOSS
[354606 bytes of object data]
```

