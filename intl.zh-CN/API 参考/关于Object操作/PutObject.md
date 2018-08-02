# PutObject {#reference_l5p_ftw_tdb .reference}

PutObject接口用于上传文件。

## 请求语法 {#section_ald_lkw_bz .section}

```
PUT /ObjectName HTTP/1.1
Content-Length：ContentLength
Content-Type: ContentType
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求Header {#section_y1z_lkw_bz .section}

|名称|类型|描述|
|--|--|--|
|Cache-Control|字符串|指定该Object被下载时的网页的缓存行为；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

|
|Content-Disposition|字符串|指定该Object被下载时的名称；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

|
|Content-Encoding|字符串|指定该Object被下载时的内容编码格式；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

|
|Content-Md5|字符串|根据协议RFC 1864对消息内容（不包括头部）计算Md5值获得128比特位数字，对该数字进行base64编码为一个消息的Content-Md5值。该请求头可用于消息合法性的检查（消息内容是否与发送时一致）。虽然该请求头是可选项，OSS建议用户使用该请求头进行端到端检查。 默认值：无

限制：无

|
|Expires|字符串|过期时间；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

**说明：** OSS不会对这个值进行限制和验证。

|
|x-oss-server-side-encryption|字符串|指定oss创建object时的服务器端加密编码算法。 合法值：AES256或 KMS

**说明：** 您需要购买KMS套件，才可以使用KMS加密算法，否则会报KmsServiceNotEnabled错误码

|
|x-oss-object-acl|字符串|指定oss创建object时的访问权限。 合法值：public-read，private，public-read-write

|

## 细节分析 {#section_fcx_vkw_bz .section}

-   如果用户上传了Content-Md5请求头，OSS会计算body的Content-Md5并检查一致性，如果不一致，将返回InvalidDigest错误码。
-   如果请求头中的“Content-Length”值小于实际请求体（body）中传输的数据长度，OSS仍将成功创建文件；但Object大小只等于“Content-Length”中定义的大小，其他数据将被丢弃。
-   如果试图添加的Object的同名文件已经存在，并且有访问权限。新添加的文件将覆盖原来的文件，成功返回200 OK。
-   如果在PutObject的时候，携带以x-oss-meta-为前缀的参数，则视为user meta，比如x-oss-meta-location。一个Object可以有多个类似的参数，但所有的user meta总大小不能超过8k。
-   如果Header不是[chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1)编码方式，且没有加入Content length参数，会返回411 Length Required错误。错误码：MissingContentLength。
-   如果设定了长度，但是没有发送消息Body，或者发送的body大小小于给定大小，服务器会一直等待，直到time out，返回400 Bad Request消息。错误码：RequestTimeout。
-   如果试图添加的Object所在的Bucket不存在，返回404 Not Found错误。错误码：NoSuchBucket。
-   如果试图添加的Object所在的Bucket没有访问权限，返回403 Forbidden错误。错误码：AccessDenied。
-   如果添加文件长度超过5G，返回错误消息400 Bad Request。错误码：InvalidArgument。
-   如果传入的Object key长度大于1023字节，返回400 Bad Request。错误码：InvalidObjectName。
-   PUT一个Object的时候，OSS支持5个 HTTP [RFC2616](https://www.ietf.org/rfc/rfc2616.txt)协议规定的Header 字段：Cache-Control、Expires、Content-Encoding、Content-Disposition、Content-Type。如果上传Object时设置了这些Header，则这个Object被下载时，相应的Header值会被自动设置成上传时的值。
-   如果上传Object时指定了x-oss-server-side-encryption Header，则必须设置其值为AES256，否则会返回400和相应错误提示：InvalidEncryptionAlgorithmError。指定该Header后，在响应头中也会返回该Header，OSS会对上传的Object进行加密编码存储，当这个Object被下载时，响应头中会包含x-oss-server-side-encryption，值被设置成该Object的加密算法。

## 常见问题 {#section_w35_wkw_bz .section}

-   **Content-Md5计算方式错误**

    以上传的内容为 `0123456789` 为例，计算这个字符串的Content-Md5。


以上传的内容为“123456789”来说，计算这个字符串的Content-Md5 正确的计算方式：标准中定义的算法为： 先计算Md5加密的二进制数组（128位），再对这个二进制进行base64编码（而不是对32位字符串编码）。 以Python为例子，正确计算的代码为：

```

>>> import base64,hashlib
>>> hash = hashlib.md5()
>>> hash.update("0123456789")
>>> base64.b64encode(hash.digest())
'eB5eJF1ptWaXm4bijSPyxw=='
```

需要注意正确的是：`hash.digest()`，计算出进制数组（128位）`>>> hash.digest() 'x\x1e^$]i\xb5f\x97\x9b\x86\xe2\x8d#\xf2\xc7'`。常见错误是直接对计算出的32位字符串编码进行base64编码。 例如，错误的是：`hash.hexdigest()`，计算得到可见的32位字符串编码`>>> hash.hexdigest() '781e5e245d69b566979b86e28d23f2c7'` 错误的Md5值进行base64编码后的结果： `>>> base64.b64encode(hash.hexdigest()) 'NzgxZTVlMjQ1ZDY5YjU2Njk3OWI4NmUyOGQyM2YyYzc='`

## 示例 {#section_orz_dlw_bz .section}

**请求示例：**

```
PUT /oss.jpg HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Cache-control: no-cache
Expires: Fri, 28 Feb 2012 05:38:42 GMT
Content-Encoding: utf-8
Content-Disposition: attachment;filename=oss_download.jpg
Date: Fri, 24 Feb 2012 06:03:28 GMT
Content-Type: image/jpg
Content-Length: 344606
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk=

[344606 bytes of object data]

```

**返回示例：**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Sat, 21 Nov 2015 18:52:34 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5650BD72207FB30443962F9A
x-oss-bucket-version: 1418321259
ETag: "A797938C31D59EDD08D86188F6D5B872"
```

