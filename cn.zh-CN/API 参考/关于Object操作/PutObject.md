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
|Authorization|字符串|表示请求本身已被授权；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

|
|Cache-Control|字符串|指定该Object被下载时的网页的缓存行为；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

|
|Content-Disposition|字符串|指定该Object被下载时的名称；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

|
|Content-Encoding|字符串|指定该Object被下载时的内容编码格式；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

|
|Content-Md5|字符串|根据协议RFC 1864对消息内容（不包括头部）计算Md5值获得128比特位数字，对该数字进行base64编码作为一个消息的Content-Md5值。该请求头可用于消息合法性的检查（消息内容是否与发送时一致）。虽然该请求头是可选项，OSS建议用户使用该请求头进行端到端检查。 默认值：无

限制：无

|
|Content-Length|字符串|请求头中的Content-Length值小于实际请求体（Body）中传输的数据长度，OSS仍将成功创建文件；但Object大小只等于Content-Length中定义的大小，其他数据将被丢弃。|
|ETag|字符串|ETag \(entity tag\) 在每个Object生成的时候被创建，用于标示一个Object的内容。对于Put Object请求创建的Object，ETag值是其内容的MD5值；对于其他方式创建的Object，ETag值是其内容的UUID。ETag值可以用于检查Object内容是否发生变化。不建议用户使用ETag来作为Object内容的MD5校验数据完整性。默认值：无

|
|Expires|字符串|过期时间；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

**说明：** OSS不会对这个值进行限制和验证。

|
|x-oss-server-side-encryption|字符串|指定OSS创建Object时的服务器端加密编码算法。 合法值：AES256或 KMS

**说明：** 您需要购买KMS套件，才可以使用KMS加密算法，否则会报KmsServiceNotEnabled错误码

|
|x-oss-server-side-encryption-key-id|字符串|表示KMS托管的用户主密钥。该参数在x-oss-server-side-encryption为KMS时有效。

|
|x-oss-object-acl|字符串|指定OSS创建Object时的访问权限。 合法值：public-read，private，public-read-write

|
|x-oss-storage-class|字符串|指定Object的存储类型。取值：

-   Standard
-   IA
-   Archive

支持的接口：PutObject、InitMultipartUpload、AppendObject、 PutObjectSymlink、CopyObject。

**说明：** 

-   如果StorageClass的值不合法，返回400 错误。错误码：InvalidArgument。
-   对于任意存储类型Bucket，若上传Object时指定该值，则此次上传的Object将存储为指定的类型。例如，在IA类型的Bucket中上传Object时，若指定x-oss-storage-class为Standard，则该Object直接存储为Standard。

|

## 细节分析 {#section_fcx_vkw_bz .section}

-   对于试图添加的Object：
    -   所在的Bucket没有访问权限，返回403 Forbidden错误。错误码：AccessDenied。
    -   已经存在同名文件，并且有访问权限。新添加的文件将覆盖原来的文件，成功返回200 OK。
    -   所在的Bucket不存在，返回404 Not Found错误。错误码：NoSuchBucket。
-   如果传入的Object key长度大于1023字节，返回400 Bad Request。错误码：InvalidObjectName。
-   Content length
    -   如果请求头中的Content-Length小于实际请求体（Body）中传输的数据长度，OSS仍将成功创建文件，但Object大小只等于Content-Length中定义的大小，其他数据将被丢弃。
    -   如果添加文件长度超过5G，返回400 Bad Request错误。错误码：InvalidArgument。
    -   如果设定了长度，但是没有发送消息Body，或者发送的Body大小小于给定大小，服务器会一直等待，直到time out，返回400 Bad Request错误。错误码：RequestTimeout。
-   HTTP Header
    -   OSS支持 HTTP 协议规定的5个Header 字段：Cache-Control、Expires、Content-Encoding、Content-Disposition、Content-Type。如果上传Object时设置了这些Header，则该Object被下载时，相应的Header值会被自动设置成上传时的值。Hearder 字段详情请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。
    -   如果Header不是[chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1)编码方式，且没有加入Content length参数，将返回411 Length Required错误。错误码：MissingContentLength。
    -   PutObject时指定了x-oss-server-side-encryption Header，则必须设置其值为AES256，否则会返回400 Bad Request错误。错误码：InvalidEncryptionAlgorithmError。指定该Header后，在响应头中也会返回该Header，OSS会对上传的Object进行加密编码存储，当该Object被下载时，响应头中会包含x-oss-server-side-encryption，值被设置成该Object的加密算法。
    -   PutObject时，携带以x-oss-meta-为前缀的参数，则视为 user meta，比如x-oss-meta-location。一个Object可以有多个类似的参数，但所有的 user meta总大小不能超过8KB。User meta支持短横线（-）、空格 、双引号、 数字、英文字母（a-z, A-Z），不支持下划线（\_）在内的其他字符。

## 常见问题 {#section_w35_wkw_bz .section}

Content-Md5计算方式错误

标准中定义的算法为： 先计算Md5加密的二进制数组（128位），再对这个二进制进行base64编码（而不是对32位字符串编码）。

以上传的内容`0123456789`为例，计算该字符串的Content-Md5。

用Python来实现，正确计算的代码为：

```

>>> import base64,hashlib
>>> hash = hashlib.md5()
>>> hash.update("0123456789")
>>> base64.b64encode(hash.digest())
'eB5eJF1ptWaXm4bijSPyxw=='
```

**说明：** 

正确的计算方式为：`hash.digest()`，计算出进制数组（128位）`>>> hash.digest() 'x\x1e^$]i\xb5f\x97\x9b\x86\xe2\x8d#\xf2\xc7'`。

常见错误是直接对计算出的32位字符串编码进行base64编码。 例如：`hash.hexdigest()`，计算得到可见的32位字符串编码`>>> hash.hexdigest() '781e5e245d69b566979b86e28d23f2c7'` 。错误的Md5值进行base64编码后的结果为： `>>> base64.b64encode(hash.hexdigest()) 'NzgxZTVlMjQ1ZDY5YjU2Njk3OWI4NmUyOGQyM2YyYzc='`

## 示例 {#section_orz_dlw_bz .section}

-   示例1

    简单上传的请求示例：

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

    响应示例：

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

-   示例2

    带有归档存储类型的请求示例：

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

    返回示例：

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

-   **说明：** 更多Put Object示例代码请参见[Python SDK上传文件](../../../../intl.zh-CN/SDK 参考/Python/上传文件/概述.md#)。


