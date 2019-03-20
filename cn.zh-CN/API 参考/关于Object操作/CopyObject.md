# CopyObject {#reference_mvx_xxc_5db .reference}

CopyObject接口用于在存储空间（Bucket ） 内或同地域的Bucket之间拷贝文件（Object）。使用CopyObject接口可以发送一个Put请求给OSS，OSS会自动判断出这是一个拷贝操作，并直接在服务器端执行该操作。

## 使用限制 {#section_c54_3q5_vgb .section}

-   CopyObject接口仅支持拷贝小于1 GB的Object。如果要拷贝大于1 GB的Object，您可以使用[UploadPartCopy](cn.zh-CN/API 参考/关于MultipartUpload的操作/UploadPartCopy.md#)接口进行操作。
-   CopyObject接口支持修改Object的元数据\(源Object与目标Object一致\)，最大可支持修改48.8 TB的Object。
-   使用此接口须对源Object有读权限。
-   源Object和目标Object必须属于同一地域。
-   不能拷贝通过追加上传方式产生的Object。
-   如果源Object为软链接，则只拷贝软链接，无法拷贝软链接指向的文件内容。

## 计量计费 {#section_efv_pq5_vgb .section}

-   单次使用CopyObject接口会对源Object所在的Bucket增加一次Get请求次数。
-   单次使用CopyObject接口会对目标Object所在的Bucket增加一次Put请求次数。
-   使用CopyObject接口会对目标Object所在的Bucket增加相应的存储量。
-   使用CopyObject接口更改Object存储类型涉及到数据覆盖，如果IA或Archive类型的Object分别在创建后30和60天内被覆盖，则它们会产生提前删除费用。比如，IA类型Object创建10天后，被覆盖成Archive或Standard，则会产生20天的提前删除费用。

## 请求语法 {#section_jzf_fmr_xdb .section}

```
PUT /DestObjectName HTTP/1.1
Host: DestBucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-copy-source: /SourceBucketName/SourceObjectName
```

## 请求头 {#section_vdd_klw_bz .section}

**说明：** 拷贝操作涉及到的请求头都是以x-oss-开头，所以所有请求头都要加到签名字符串中。

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|x-oss-copy-source|字符串|是| 指定拷贝的源地址。

 默认值：无

 |
|x-oss-copy-source-if-match|字符串|否| 如果源Object的ETag值和您提供的ETag相等，则执行拷贝操作，并返回200 OK；否则返回412 Precondition Failed错误码（预处理失败）。

 默认值：无

 |
|x-oss-copy-source-if-none-match|字符串|否| 如果源Object的ETag值和您提供的ETag不相等，则执行拷贝操作，并返回200 OK；否则返回304 Not Modified错误码（预处理失败）。

 默认值：无

 |
|x-oss-copy-source-if-unmodified-since|字符串|否| 如果指定的时间等于或者晚于文件实际修改时间，则正常拷贝文件，并返回200 OK；否则返回412 Precondition Failed错误码（预处理失败）。

 默认值：无

 |
|x-oss-copy-source-if-modified-since|字符串|否| 如果源Object在用户指定的时间以后被修改过，则执行拷贝操作；否则返回304 Not Modified错误码（预处理失败）。

 默认值：无

 |
|x-oss-metadata-directive|字符串|否| 指定如何设置目标Object的元信息。取值如下：

 -   COPY （默认值）

复制源Object的元数据到目标Object。

**说明：** 不会复制源Object的x-oss-server-side-encryption。目标Object是否进行服务器端加密编码只根据当前拷贝操作是否指定了x-oss-server-side-encryption来决定。

-   REPLACE

忽略源Object的元数据，直接采用请求中指定的元数据。


**说明：** 如果拷贝操作的源Object地址和目标Object地址相同，则无论x-oss-metadata-directive为何值，都会直接替换源Object的元数据。

 |
|x-oss-server-side-encryption|字符串|否| 指定OSS创建目标Object时，服务器端熵编码加密算法 。

 取值：AES256、KMS（您需要购买 KMS 套件，才可以使用KMS加密算法，否则会返回KmsServiceNotEnabled错误码）

 **说明：** 

-   如果拷贝操作中未指定x-oss-server-side-encryption，则无论源Object是否进行过服务器端加密编码，拷贝之后的目标Object都未进行服务器端加密编码。
-   如果拷贝操作中指定了x-oss-server-side-encryption，则无论源Object是否进行过服务器端加密编码，拷贝之后的目标Object都会进行服务器端加密编码。并且拷贝操作的响应头中会包含x-oss-server-side-encryption，值为目标Object的加密算法。在目标Object被下载时，响应头中也会包含x-oss-server-side-encryption，值为该Object的加密算法。

 |
|x-oss-server-side-encryption-key-id|字符串|否| 表示KMS托管的用户主密钥。

 该参数在x-oss-server-side-encryption为KMS时有效。

 |
|x-oss-object-acl|字符串|否| 指定OSS创建目标Object时的访问权限。

 取值：public-read、private、public-read-write、default

 |
|x-oss-storage-class|字符串|否| 指定Object的存储类型。

 取值：Standard、IA、Archive

 支持的接口：PutObject、InitMultipartUpload、AppendObject、 PutObjectSymlink、CopyObject

 **说明：** 

-   IA与Archive类型的单个文件如不足64 KB，会按64 KB 计量计费。建议您在使用CopyObject接口时不要将Object的存储类型指定为IA或Archive。
-   对于任意存储类型的Bucket，若上传Object时指定此参数，则此次上传的Object将存储为指定的类型。例如，在IA类型的Bucket中上传Object时，若指定x-oss-storage-class为Standard，则该Object直接存储为Standard。
-   更改Object存储类型涉及到数据覆盖，如果IA或Archive类型Object分别在创建后30和60天内被覆盖，则它们会产生提前删除费用。

 |

## 响应元素 {#section_tvz_qlw_bz .section}

|名称|类型|描述|
|:-|:-|:-|
|CopyObjectResult|字符串| CopyObject的结果。

 默认值：无

 |
|ETag|字符串| 目标Object的ETag值。

 父元素：CopyObjectResult

 |
|LastModified|字符串| 目标Object最后更新时间。

 父元素：CopyObjectResult

 |

## 示例 {#section_osk_5lw_bz .section}

**示例1**

请求示例

```
PUT /copy_oss.jpg HTTP/1.1 
Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
Date: Fri, 24 Feb 2012 07:18:48 GMT 
x-oss-storage-class: Archive
x-oss-copy-source: /oss-example/oss.jpg 
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:gmnwPKuu20LQEjd+iPkL259A+n0=
```

返回示例

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Content-Type: application/xml
Content-Length: 193
Connection: keep-alive
Date: Fri, 24 Feb 2012 07:18:48 GMT
Server: AliyunOSS
<?xml version="1.0" encoding="UTF-8"?>
<CopyObjectResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
 <LastModified>Fri, 24 Feb 2012 07:18:48 GMT</LastModified>
 <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE"</ETag>
</CopyObjectResult>
```

**示例 2**

请求示例

```
PUT /test%2FAK.txt HTTP/1.1
Host: tesx.oss-cn-zhangjiakou.aliyuncs.com
Accept-Encoding: identity
User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
Accept: */*
Connection: keep-alive
x-oss-copy-source: /test/AK.txt
date: Fri, 28 Dec 2018 09:41:55 GMT
authorization: OSS qn6qrrqxo2oawuk53otfjbyc:gmnwPKuu20LQEjd+iPkL259A+n0=
Content-Length: 0
```

返回示例

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Fri, 28 Dec 2018 09:41:56 GMT
Content-Type: application/xml
Content-Length: 184
Connection: keep-alive
x-oss-request-id: 5C25EFE4462CE00EC6D87156
ETag: "F2064A169EE92E9775EE5324D0B1682E"
x-oss-hash-crc64ecma: 12753002859196105360
x-oss-server-time: 150

<?xml version="1.0" encoding="UTF-8"?>
<CopyObjectResult>
  <ETag>"F2064A169EE92E9775EE5324D0B1682E"</ETag>
  <LastModified>2018-12-28T09:41:56.000Z</LastModified>
</CopyObjectResult>
```

**说明：** x-oss-hash-crc64ecma表明Object的64位CRC值。该64位CRC值根据[ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm)标准计算得出。进行CopyObject操作时，生成的Object不保证具有此值。

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/管理文件/拷贝文件.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/管理文件/拷贝文件.md)
-   [PHP](../../../../../cn.zh-CN/SDK 参考/PHP/管理文件/拷贝文件.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/管理文件/拷贝文件.md)
-   [C](../../../../../cn.zh-CN/SDK 参考/C/管理文件/拷贝文件.md)
-   [.NET](../../../../../cn.zh-CN/SDK 参考/.NET/管理文件/删除文件.md)
-   [Android](../../../../../cn.zh-CN/SDK 参考/Android/管理文件/拷贝文件.md)
-   [iOS](../../../../../cn.zh-CN/SDK 参考/iOS/管理文件/概述.md)
-   [Node.js](../../../../../cn.zh-CN/SDK 参考/Node.js/管理文件/概述.md)
-   [Ruby](../../../../../cn.zh-CN/SDK 参考/Ruby/管理文件.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|InvalidArgument|400|x-oss-storage-class等参数的值无效。|
|Precondition Failed|412| -   指定了x-oss-copy-source-if-match请求头，但源Object的ETag值和您提供的ETag不相等。
-   指定了x-oss-copy-source-if-unmodified-since请求头，且指定的时间早于文件实际修改时间。

 |
|Not Modified|304| -   指定了x-oss-copy-source-if-none-match请求头，且源Object的ETag值和您提供的ETag相等。
-   指定了x-oss-copy-source-if-modified-since请求头，但源Object在指定的时间后没被修改过。

 |
|KmsServiceNotEnabled|403|将x-oss-server-side-encryption指定为KMS，但没有预先购买KMS套件。|

