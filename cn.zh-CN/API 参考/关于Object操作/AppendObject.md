# AppendObject {#reference_fvf_xld_5db .reference}

AppendObject接口用于以追加写的方式上传文件（Object）。通过AppendObject操作创建的Object类型为Appendable Object，而通过PutObject上传的Object是Normal Object。

## 使用限制 {#section_y31_h4r_xdb .section}

-   AppendObject产生的Object大小不得超过5 GB。
-   处于WORM保护期的Object不支持AppendObject操作。
-   AppendableObject不支持指定CMK ID进行服务端KMS加密。

## 请求语法 {#section_n23_kpw_bz .section}

```
POST /ObjectName?append&position=Position HTTP/1.1
Content-Length：ContentLength
Content-Type: ContentType
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头 {#section_x4j_lpw_bz .section}

**说明：** append和position均为CanonicalizedResource，需要包含在签名中。

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|Append|字符串|是| 用于指定这是一个AppendObject操作。

 |
|Position|字符串|是| 用于指定从何处进行追加。

 首次追加操作的position必须为0，后续追加操作的position是Object的当前大小。例如，第一次AppendObject请求指定position值为0，content-length是65536，则第二次AppendObject需要指定position为65536。

 每次操作成功后，响应消息头x-oss-next-append-position也会标明下一次追加的position。

 **说明：** 

-   当position值为0时，如果没有同名Object存在，那么AppendObject可以和PutObject请求一样，设置x-oss-server-side-encryption等请求头。如果加入了正确的x-oss-server-side-encryption头，那么后续的AppendObject响应头部也会包含x-oss-server-side-encryption头。后续如需要更改元数据，可以使用CopyObject接口。
-   每次执行AppendObject都会更新该Object的最后修改时间。
-   在position值正确的情况下，对已存在的Appendable Object追加一个大小为0的内容，该操作不会改变Object的状态。

 |
|Cache-Control|字符串|否| 指定该Object的网页缓存行为。详情参考[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。

 默认值：无

 |
|Content-Disposition|字符串|否| 指定该Object被下载时的名称。详情参考[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。

 默认值：无

 |
|Content-Encoding|字符串|否| 指定该Object的内容编码格式。详情参考[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。

 默认值：无

 |
|Content-MD5|字符串|否| Content-MD5是一串由MD5算法生成的值，该请求头用于检查消息内容是否与发送时一致。

 获取Content-MD5值：对消息内容（不包括头部）执行MD5算法获得128比特位数字，然后对该数字进行base64编码。

 默认值：无

 限制：无

 |
|Expires|GMT|否| 过期时间。详情参考[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。

 默认值：无

 |
|x-oss-server-side-encryption|字符串|否| 指定服务器端加密编码算法。

 合法值：AES256 或 KMS

 **说明：** 您需要购买KMS套件，才可以使用KMS加密算法，否则会返回KmsServiceNotEnabled。

 |
|x-oss-object-acl|字符串|否| 指定Object的访问权限。

 合法值：public-read、private、public-read-write

 |
|x-oss-storage-class|字符串|否| 指定Object的存储类型。

 取值：Standard、IA、Archive

 支持的接口：PutObject、InitMultipartUpload、AppendObject、 PutObjectSymlink、CopyObject。

 **说明：** 

-   对于任意存储类型的Bucket，若上传Object时指定此参数，则此次上传的Object将存储为指定的类型。例如，在IA类型的Bucket中上传Object时，若指定x-oss-storage-class为Standard，则该Object直接存储为Standard。
-   该值仅在首次执行AppendObject操作时有效，后续追加时不会生效。

 |

**说明：** 

-   使用AppendObject接口时，如果配置以x-oss-meta-\*为前缀的参数，则该参数视为元数据，例如x-oss-meta-location。一个Object可以有多个类似的参数，但所有的元数据总大小不能超过8KB。元数据支持短横线（-）、数字、英文字母（a-z），英文字符的大写字母会被转成小写字母，不支持下划线（\_）在内的其他字符。

    **说明：** 创建AppendObject时可以添加x-oss-meta-\*，继续追加时不可以携带此参数。


## 响应头 {#section_xbv_rpw_bz .section}

|响应消息头|类型|描述|
|-----|--|--|
|x-oss-next-append-position|64位整型| 下一次请求应当提供的position，即当前Object大小。

 当AppendObject执行成功，或是因position和Object大小不匹配而引起的409错误时，会返回此消息头。

 |
|x-oss-hash-crc64ecma|64位整型| 表明Object的64位CRC值。该64位CRC由[ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm)标准计算得出。

 |

## CRC64的计算方式 {#section_xb5_xpw_bz .section}

Appendable Object的CRC采用[ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm)标准。您可以使用以下方法进行计算：

用Boost CRC模块的方式计算

```
typedef boost::crc_optimal<64, 0x42F0E1EBA9EA3693ULL, 0xffffffffffffffffULL, 0xffffffffffffffffULL, true, true> boost_ecma;

uint64_t do_boost_crc(const char* buffer, int length)
{
    boost_ecma crc;
    crc.process_bytes(buffer, length);
    return crc.checksum();
}
```

用Python crcmod的方式计算

```
do_crc64 = crcmod.mkCrcFun(0x142F0E1EBA9EA3693L, initCrc=0L, xorOut=0xffffffffffffffffL, rev=True)

print do_crc64(“123456789”)
```

## 示例 {#section_phh_cqw_bz .section}

**请求示例**

```
POST /oss.jpg?append&position=0 HTTP/1.1 
Host: oss-example.oss.aliyuncs.com 
Cache-control: no-cache 
Expires: Wed, 08 Jul 2015 16:57:01 GMT 
Content-Encoding: utf-8 
x-oss-storage-class: Archive
Content-Disposition: attachment;filename=oss_download.jpg 
Date: Wed, 08 Jul 2015 06:57:01 GMT 
Content-Type: image/jpg 
Content-Length: 1717 
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk=  
[1717 bytes of object data]
```

**返回示例**

```
HTTP/1.1 200 OK
Date: Wed, 08 Jul 2015 06:57:01 GMT
ETag: "0F7230CAA4BE94CCBDC99C5500000000"
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
x-oss-hash-crc64ecma: 14741617095266562575
x-oss-next-append-position: 1717
x-oss-request-id: 559CC9BDC755F95A64485981
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/上传文件/追加上传.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/上传文件/追加上传.md)
-   [PHP](../../../../../cn.zh-CN/SDK 参考/PHP/上传文件/追加上传.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/上传文件/追加上传.md)
-   [C](../../../../../cn.zh-CN/SDK 参考/C/上传文件/追加上传.md)
-   [.NET](../../../../../cn.zh-CN/SDK 参考/.NET/上传文件/追加上传.md)
-   [Android](../../../../../cn.zh-CN/SDK 参考/Android/上传文件/追加上传.md)
-   [iOS](../../../../../cn.zh-CN/SDK 参考/iOS/上传文件/概述.md)
-   [Ruby](../../../../../cn.zh-CN/SDK 参考/Ruby/上传文件.md)

## 和其他操作的关系 {#section_vgd_wpw_bz .section}

|其他操作|关系描述|
|:---|:---|
|[PutObject](cn.zh-CN/API 参考/关于Object操作/PutObject.md)|如果对一个已经存在的Appendable Object进行PutObject操作，该Appendable Object会被新的Object覆盖，类型变为Normal Object。|
|[HeadObject](cn.zh-CN/API 参考/关于Object操作/HeadObject.md)|对Appendable Object执行HeadObject操作会返回x-oss-next-append-position、x-oss-hash-crc64ecma以及x-oss-object-type。Appendable Object的x-oss-object-type为Appendable。|
|[GetBucket](cn.zh-CN/API 参考/关于Object操作/GetObject.md)|GetBucket请求的响应中，会把Appendable Object的Type设为Appendable。|
|[CopyObject](cn.zh-CN/API 参考/关于Object操作/CopyObject.md)|不能使用CopyObject来拷贝一个Appendable Object，也不能使用CopyObject改变Appendable Object的服务器端加密属性。可以使用CopyObject来改变自定义的元信息。|

## 错误码 {#section_oj4_zfn_qgb .section}

|错误代码|HTTP 状态码|说明|
|:---|:-------|:-|
|ObjectNotAppendable|409|对一个非Appendable Object进行AppendObject操作。|
|PositionNotEqualToLength|409| -   position的值和当前Object的长度不一致。

**说明：** 

    -   您可以通过响应头x-oss-next-append-position得到下一次position，并再次进行请求。由于并发的关系，即使把position的值设为了x-oss-next-append-position，该请求依然可能因为PositionNotEqualToLength而失败。
-   position值为0时，如果没有同名Appendable Object，或者同名Appendable Object长度为0，该请求成功；其他情况均视为position和Object长度不匹配的情形，返回此错误码。

 |
|InvalidArgument|400|x-oss-storage-class等值无效。|

