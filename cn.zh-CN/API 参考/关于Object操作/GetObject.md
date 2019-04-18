# GetObject {#reference_ccf_rgd_5db .reference}

GetObject接口用于获取某个文件（Object） 。此操作需要对该Object有读权限。

**说明：** 如果Object类型为归档类型，需要先完成RestoreObject请求且该请求不能超时。

## 请求语法 {#section_pjt_dmw_bz .section}

```
GET /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
Range: bytes=ByteRange(可选)
```

## 请求头 {#section_gsg_gmw_bz .section}

**说明：** 

-   OSS支持在GET请求中通过请求头来自定义响应头，但只有请求成功（即返回码为 200 OK ）才会将响应头的值设置成GET请求头中指定的值。
-   OSS不支持在匿名访问的GET请求中自定义响应头。
-   用户发送的GET请求必须携带签名。

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|response-content-type|字符串|否| 指定OSS返回请求的content-type头。

 默认值：无

 |
|response-content-language|字符串|否| 指定OSS返回请求的content-language头。

 默认值：无

 |
|response-expires|字符串|否| 指定OSS返回请求的expires头。

 默认值：无

 |
|response-cache-control|字符串|否| 指定OSS返回请求的cache-control头。

 默认值：无

 |
|response-content-disposition|字符串|否| 指定OSS返回请求的content-disposition头。

 默认值：无

 |
|response-content-encoding|字符串|否| 指定OSS返回请求的content-encoding头。

 默认值：无

 |
|Range|字符串|否| 指定文件传输的范围。

 默认值：无

 -   如果指定的范围符合规范，返回消息中会包含整个Object的大小和此次返回的范围。例如：Content-Range: bytes 0-9/44，表示整个Object大小为44，此次返回的范围为0-9。
-   如果指定的范围不符合范围规范，则传送整个Object，并且不在结果中提及Content-Range。

 |
|If-Modified-Since|字符串|否| 如果指定的时间早于实际修改时间或指定的时间不符合规范，会直接返回Object，并返回200 OK；否则返回304 Not Modified。

 默认值：无

 时间格式：GMT，例如`Fri, 13 Nov 2015 14:47:53 GMT`

 |
|If-Unmodified-Since|字符串|否| 如果指定的时间等于或者晚于Object实际修改时间，则正常传输Object，并返回200 OK；否则返回412 Precondition Failed。

 默认值：无

 时间格式：GMT，例如`Fri, 13 Nov 2015 14:47:53 GMT`

 If-Modified-Since和If-Unmodified-Since可以同时使用。

 |
|If-Match|字符串|否| 如果传入期望的ETag和Object的ETag匹配，则正常传输Object，并返回200 OK；否则返回412 Precondition Failed。

 默认值：无

 |
|If-None-Match|字符串|否| 如果传入的ETag值和Object的ETag不匹配，则正常传输Object，并返回200 OK；否则返回304 Not Modified。

 默认值：无

 If-Match和If-None-Match可以同时使用。

 |
|Accept-Encoding|字符串|否| 指定客户端的编码类型。

 如果需要对返回内容进行GZIP压缩传输，您需要在请求头中以显示方式加入Accept-Encoding:gzip。OSS会根据Object的Content-Type和Object大小（不小于1 KB）判断是否返回经过GZIP压缩的数据。

 **说明：** 

-   如果采用了GZIP压缩则不会附带ETag信息。
-   目前OSS支持GZIP压缩的Content-Type为HTML、Javascript、CSS、XML、RSS、JSON。

 |

## 响应头 {#section_ump_3hn_xgb .section}

**说明：** 如果Object类型为符号链接，会返回目标Object的内容。响应头中`Content-Length`、`ETag`、`Content-Md5` 均为目标Object的元数据；`Last-Modified`取目标Object和符号链接对应的最大值（即在两者中取较晚的时间）；其他均为符号链接的元数据。

|名称|类型|描述|
|--|--|--|
|x-oss-server-side-encryption|字符串|若Object在服务器端采用熵编码加密存储，使用GET请求时，系统会自动解密返回给用户，并且在响应头中返回x-oss-server-side-encryption，表明该Object的服务器端加密算法。|
|x‑oss‑tagging‑count|字符串|对象关联的标签的个数。仅当用户有读取标签权限时返回|

## 示例 {#section_vrp_zmw_bz .section}

**简单的GET请求示例**

```
GET /oss.jpg HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 06:38:30 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:UNQDb7GapEgJkcde6OhZ9J*****
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 3a8f-2e2d-7965-3ff9-51c875b*****
x-oss-object-type: Normal
Date: Fri, 24 Feb 2012 06:38:30 GMT
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
ETag: "5B3C1A2E0563E1B002CC607C*****"
Content-Type: image/jpg
Content-Length: 344606
Server: AliyunOSS
[344606 bytes of object data]
```

**带有Range参数的请求示例**

```
GET //oss.jpg HTTP/1.1
Host:oss-example. oss-cn-hangzhou.aliyuncs.com
Date: Fri, 28 Feb 2012 05:38:42 GMT
Range: bytes=100-900
Authorization: OSS qn6qrrqxo2oawuk5jbyc:qZzjF3DUtd+yK16BdhGtFcC*****
```

**返回示例**

```
HTTP/1.1 206 Partial Content
x-oss-request-id: 28f6-15ea-8224-234e-c0ce407*****
x-oss-object-type: Normal
Date: Fri, 28 Feb 2012 05:38:42 GMT
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
ETag: "5B3C1A2E05E1B002CC607C*****"
Accept-Ranges: bytes
Content-Range: bytes 100-900/344606
Content-Type: image/jpg
Content-Length: 801
Server: AliyunOSS
[801 bytes of object data]
```

**带自定义返回消息头的请求示例**

```
GET /oss.jpg?response-expires=Thu%2C%2001%20Feb%202012%2017%3A00%3A00%20GMT& response-content-type=text&response-cache-control=No-cache&response-content-disposition=attachment%253B%2520filename%253Dtesting.txt&response-content-encoding=utf-8&response-content-language=%E4%B8%AD%E6%96%87 HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com:
Date: Fri, 24 Feb 2012 06:09:48 GMT
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC75A644*****
x-oss-object-type: Normal
Date: Fri, 24 Feb 2012 06:09:48 GMT 
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
ETag: "5B3C1A2E053D1B002CC607*****"
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

**Object类型为符号链接的请求示例**

```
GET /link-to-oss.jpg HTTP/1.1
Accept-Encoding: identity
Date: Tue, 08 Nov 2016 03:17:58 GMT
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS qn6qrrqxok53otfjbyc:qZzjF3DUtd+yK16BdhGtFc*****
```

**返回示例**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Tue, 08 Nov 2016 03:17:58 GMT
Content-Type: application/octet-stream
Content-Length: 20
Connection: keep-alive
x-oss-request-id: 582143E6A212AD*****
Accept-Ranges: bytes
ETag: "8086265EFC021F9A2F09BF4****"
Last-Modified: Tue, 08 Nov 2016 03:17:58 GMT
x-oss-object-type: Symlink
Content-MD5: gIYmXvwCEe0fmi8Jv0Y****
```

**Restore操作已经完成的请求示例**

```
GET /oss.jpg HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 09:38:30 GMT
Authorization: OSS qn6qrrqxo2o***k53otfjbyc:zUglwRPGkbByZxm1+y4eyu+*****
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 58F723829F29F18D7F00*****
x-oss-object-type: Normal
x-oss-restore: ongoing-request="false", expiry-date="Sun, 16 Apr 2017 08:12:33 GMT"
Date: Sat, 15 Apr 2017 09:38:30 GMT
Last-Modified: Sat, 15 Apr 2017 06:07:48 GMT
ETag: "5B3C1A2E0763E1B002CC607C*****"
Content-Type: image/jpg
Content-Length: 344606
Server: AliyunOSS
[354606 bytes of object data]
```

## SDK {#section_egl_m2c_5gb .section}

GetObject接口所对应的各语言SDK如下：

-   [Java](../../../../intl.zh-CN/SDK 参考/Java/上传文件/概述.md)
-   [Python](../../../../intl.zh-CN/SDK 参考/Python/上传文件/概述.md)
-   [PHP](../../../../intl.zh-CN/SDK 参考/PHP/上传文件/概述 .md)
-   [Go](../../../../intl.zh-CN/SDK 参考/Go/上传文件/概述.md)
-   [C](../../../../intl.zh-CN/SDK 参考/C/上传文件/概述.md)
-   [.NET](../../../../intl.zh-CN/SDK 参考/.NET/上传文件/概述.md)
-   [Node.js](../../../../intl.zh-CN/SDK 参考/Node.js/上传文件/概述.md)
-   [Browser.js](../../../../intl.zh-CN/SDK 参考/Browser.js/上传文件.md)
-   [Ruby](../../../../intl.zh-CN/SDK 参考/Ruby/上传文件.md)

## 错误码 {#section_hnc_tz5_jgb .section}

|错误码|HTTP状态码|说明|
|---|-------|--|
|NoSuchKey|404|目标Object不存在。|
|SymlinkTargetNotExist|404|Object类型为符号链接，且目标Object不存在。|
|InvalidTargetType|400|Object类型为符号链接，且目标Object类型仍为符号链接。|
|InvalidObjectState|403|下载归档类型的Object时， -   没有提交RestoreObject请求或者上一次提交RestoreObject已经超时。
-   已经提交RestoreObject请求，但数据的RestoreObject操作还没有完成。

 |
|Not Modified|304| -   指定了If-Modified-Since请求头，但源Object在指定的时间后没被修改过。
-   指定了If-None-Match请求头，且源Object的ETag值和您提供的ETag相等。

 |
|Precondition Failed|412| -   指定了If-Unmodified-Since，但指定的时间早于Object实际修改时间 。
-   指定了If-Match，但源Object的ETag值和您提供的ETag不相等。

 |

