# PutObject {#reference_l5p_ftw_tdb .reference}

PutObject接口用于上传文件（Object）。

**说明：** 

-   添加的文件大小不得超过5 GB。
-   如果已经存在同名的Object，并且有访问权限，则新添加的文件将覆盖原来的文件，并成功返回200 OK。

## 请求语法 {#section_ald_lkw_bz .section}

```
PUT /ObjectName HTTP/1.1
Content-Length：ContentLength
Content-Type: ContentType
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头 {#section_y1z_lkw_bz .section}

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|Authorization|字符串|否| 表示请求本身已被授权，详细描述参考[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。

 一般情况下Authorization是必选请求头，但是若采用URL包含签名，可以不用携带该请求头。详细描述参考[在URL中包含签名](cn.zh-CN/API 参考/访问控制/在URL中包含签名.md)。

 默认值：无

 |
|Cache-Control|字符串|否| 指定该Object被下载时网页的缓存行为，详细描述参考[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。

 默认值：无

 |
|Content-Disposition|字符串|否| 指定该Object被下载时的名称，详细描述参考[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。

 默认值：无

 |
|Content-Encoding|字符串|否| 指定该Object被下载时的内容编码格式，详细描述参考[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。

 默认值：无

 |
|Content-MD5|字符串|否| 用于检查消息内容是否与发送时一致。Content-MD5是一串由MD5算法生成的值。上传了Content-MD5请求头后，OSS会计算消息体的Content-MD5并检查一致性。

 默认值：无

 限制：无

 |
|Content-Length|字符串|否| 用于描述HTTP消息体的传输大小。

 如果请求头中的Content-Length值小于实际请求体中传输的数据大小，OSS仍将成功创建文件，但Object的大小只等于Content-Length中定义的大小，其他数据将被丢弃。

 |
|ETag|字符串|否| Object生成时会创建相应的ETag \(entity tag\) ，ETag用于标示一个Object的内容。对于PutObject请求创建的Object，ETag值是其内容的MD5值；对于其他方式创建的Object，ETag值是其内容的UUID。ETag值可以用于检查Object内容是否发生变化。不建议用户使用ETag来作为Object内容的MD5校验数据完整性。

 默认值：无

 |
|Expires|字符串|否| 过期时间，详细描述参考照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。

 默认值：无

 **说明：** OSS不会对该值进行限制和验证。

 |
|x-oss-server-side-encryption|字符串|否| 指定OSS创建Object时的服务器端加密编码算法。

 取值：AES256或 KMS（您需要购买KMS套件，才可以使用 KMS 加密算法，否则会报 KmsServiceNotEnabled 错误码）

 指定此参数后，在响应头中会返回此参数，OSS会对上传的Object进行加密编码存储。当下载该Object时，响应头中会包含x-oss-server-side-encryption，且该值会被设置成该Object的加密算法。

 |
|x-oss-server-side-encryption-key-id|字符串|否| KMS托管的用户主密钥。

 该参数在x-oss-server-side-encryption为KMS时有效。

 |
|x-oss-object-acl|字符串|否| 指定OSS创建Object时的访问权限。

 合法值：public-read，private，public-read-write

 |
|x-oss-storage-class|字符串|否| 指定Object的存储类型。

 对于任意存储类型的Bucket，若上传Object时指定此参数，则此次上传的Object将存储为指定的类型。例如，在IA类型的Bucket中上传Object时，若指定x-oss-storage-class为Standard，则该Object直接存储为Standard。

 取值：Standard、IA、Archive

 支持的接口：PutObject、InitMultipartUpload、AppendObject、 PutObjectSymlink、CopyObject。

 |

**说明：** 

-   OSS支持HTTP协议规定的5个请求头：Cache-Control、Expires、Content-Encoding、Content-Disposition、Content-Type。如果上传Object时设置了这些请求头，则该Object被下载时，相应的请求头值会被自动设置成上传时的值。
-   使用PutObject接口时，如果配置以x-oss-meta-为前缀的参数，则该参数视为元数据，例如x-oss-meta-location。一个Object可以有多个类似的参数，但所有的元数据总大小不能超过8KB。元数据支持短横线（-）、数字、英文字母（a-z），英文字符的大写字母会被转成小写字母，不支持下划线（\_）在内的其他字符。

## 示例 {#section_orz_dlw_bz .section}

**简单上传的请求示例**

```
PUT /test.txt HTTP/1.1
Host: test.oss-cn-zhangjiakou.aliyuncs.com
User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
Accept: */*
Connection: keep-alive
Content-Type: text/plain
date: Tue, 04 Dec 2018 15:56:37 GMT
authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk=
Transfer-Encoding: chunked
```

**返回示例**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Tue, 04 Dec 2018 15:56:38 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5C06A3B67B8B5A3DA422299D
ETag: "D41D8CD98F00B204E9800998ECF8427E"
x-oss-hash-crc64ecma: 0
Content-MD5: 1B2M2Y8AsgTpgAmY7PhCfg==
x-oss-server-time: 7
```

**带有归档存储类型的请求示例**

```
PUT /oss.jpg HTTP/1.1 
Host: oss-example.oss-cn-hangzhou.aliyuncs.com Cache-control: no-cache 
Expires: Fri, 28 Feb 2012 05:38:42 GMT 
Content-Encoding: utf-8
Content-Disposition: attachment;filename=oss_download.jpg 
Date: Fri, 24 Feb 2012 06:03:28 GMT 
Content-Type: image/jpg 
Content-Length: 344606 
x-oss-storage-class: Archive
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk=  

[344606 bytes of object data]
```

**返回示例**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Sat, 21 Nov 2015 18:52:34 GMT
Content-Type: image/jpg
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5650BD72207FB30443962F9A
x-oss-bucket-version: 1418321259

ETag: "A797938C31D59EDD08D86188F6D5B872"
```

## SDK {#section_egl_m2c_5gb .section}

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/上传文件/简单上传.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/上传文件/简单上传.md) 
-   [PHP](../../../../../cn.zh-CN/SDK 参考/PHP/上传文件/简单上传.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/上传文件/简单上传.md)
-   [C](../../../../../cn.zh-CN/SDK 参考/C/上传文件/简单上传.md)
-   [.NET](../../../../../cn.zh-CN/SDK 参考/.NET/上传文件/简单上传.md)
-   [Android](../../../../../cn.zh-CN/SDK 参考/Android/上传文件/简单上传.md)
-   [iOS](../../../../../cn.zh-CN/SDK 参考/iOS/上传文件/概述.md)
-   [Node.js](../../../../../cn.zh-CN/SDK 参考/Node.js/上传文件.md)
-   [Browser.js](../../../../../cn.zh-CN/SDK 参考/Browser.js/上传文件.md)
-   [Ruby](../../../../../cn.zh-CN/SDK 参考/Ruby/上传文件.md)

## 常见问题 {#section_w35_wkw_bz .section}

**如何计算Content-MD5？**

首先计算MD5加密的二进制数组（128位），然后再对这个二进制数组进行base64编码（而不是对32位字符串编码）。例如，用Python计算`0123456789`的Content-MD5，代码为：

```

>>> import base64,hashlib
>>> hash = hashlib.md5()
>>> hash.update("0123456789")
>>> base64.b64encode(hash.digest())
'eB5eJF1ptWaXm4bijSPyxw=='
```

**说明：** 

正确的计算方式为：`hash.digest()`，计算出进制数组（128位）`>>> hash.digest() 'x\x1e^$]i\xb5f\x97\x9b\x86\xe2\x8d#\xf2\xc7'`。

常见错误是直接对计算出的32位字符串编码进行base64编码。 例如：使用`hash.hexdigest()`计算方式，计算得到可见的32位字符串编码`>>> hash.hexdigest() '781e5e245d69b566979b86e28d23f2c7'` 。错误的MD5值进行base64编码后的结果为： `>>> base64.b64encode(hash.hexdigest()) 'NzgxZTVlMjQ1ZDY5YjU2Njk3OWI4NmUyOGQyM2YyYzc='`。

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|MissingContentLength|411|请求头没有采用[chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1)编码方式，且没有Content-Length参数。|
|InvalidEncryptionAlgorithmError|400|指定x-oss-server-side-encryption的值无效。有效值为AES256或KMS。|
|AccessDenied|403|对试图添加Object的Bucket没有访问权限。|
|NoSuchBucket|404|试图添加Object的Bucket不存在。|
|InvalidObjectName|400|传入的Object key长度大于1023字节。|
|InvalidArgument|400| -   添加的文件大小超过5 GB。
-   x-oss-storage-class等参数的值无效。

 |
|RequestTimeout|400|指定了Content-Length，但是没有发送消息体，或者发送的消息体小于给定大小。这种情况下服务器会一直等待，直到time out。|
|KmsServiceNotEnabled|403|将x-oss-server-side-encryption指定为KMS，但没有预先购买KMS套件。|

