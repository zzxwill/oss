# AppendObject {#reference_fvf_xld_5db .reference}

AppendObject接口用于以追加写的方式上传文件。

通过Append Object操作创建的Object类型为Appendable Object，而通过Put Object上传的Object是Normal Object。

## 请求语法 {#section_n23_kpw_bz .section}

```
POST /ObjectName?append&position=Position HTTP/1.1
Content-Length：ContentLength
Content-Type: ContentType
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求Header {#section_x4j_lpw_bz .section}

|名称|类型|描述|
|--|--|--|
|Cache-Control|字符串|指定该Object被下载时的网页的缓存行为；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

|
|Content-Disposition|字符串|指定该Object被下载时的名称；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

|
|Content-Encoding|字符串|指定该Object被下载时的内容编码格式；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

|
|Content-MD5|字符串|根据协议RFC 1864对消息内容（不包括头部）计算MD5值获得128比特位数字，对该数字进行base64编码为一个消息的Content-MD5值。该请求头可用于消息合法性的检查（消息内容是否与发送时一致）。虽然该请求头是可选项，OSS建议用户使用该请求头进行端到端检查。 默认值：无

限制：无

|
|Expires|整数|过期时间；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无

|
|x-oss-server-side-encryption|字符串|指定oss创建object时的服务器端加密编码算法。 合法值：AES256 或 KMS

**说明：** 您需要购买KMS套件，才可以使用KMS加密算法，否则会报KmsServiceNotEnabled错误码。

|
|x-oss-object-acl|字符串|指定oss创建object时的访问权限。 合法值：public-read，private，public-read-write

|

## 响应Header {#section_xbv_rpw_bz .section}

|名称|类型|描述|
|--|--|--|
|x-oss-next-append-position|64位整型|指明下一次请求应当提供的position。实际上就是当前Object长度。当Append Object成功返回，或是因position和Object长度不匹配而引起的409错误时，会包含此header。|
|x-oss-hash-crc64ecma|64位整型|表明Object的64位CRC值。该64位CRC根据[ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm)标准计算得出。|

## 和其他操作的关系 {#section_vgd_wpw_bz .section}

-   不能对一个非Appendable Object进行Append Object操作。例如，已经存在一个同名Normal Object时，Append Object调用返回409，错误码ObjectNotAppendable。
-   对一个已经存在的Appendable Object进行Put Object操作，那么该Appendable Object会被新的Object覆盖，类型变为Normal Object。
-   Head Object操作会返回x-oss-object-type，用于表明Object的类型。对于Appendable Object来说，该值为Appendable。对Appendable Object，Head Object也会返回上述的x-oss-next-append-position和x-oss-hash-crc64ecma。
-   Get Bucket（List Objects）请求的响应XML中，会把Appendable Object的Type设为Appendable
-   不能使用Copy Object来拷贝一个Appendable Object，也不能改变它的服务器端加密的属性。可以使用Copy Object来改变用户自定义元信息。

## 细节分析 {#section_jxx_wpw_bz .section}

-   URL参数append和position均为CanonicalizedResource，需要包含在签名中。
-   URL的参数必须包含append，用来指定这是一个Append Object操作。
-   URL查询参数还必须包含position，其值指定从何处进行追加。首次追加操作的position必须为0，后续追加操作的position是Object的当前长度。例如，第一次Append Object请求指定position值为0，content-length是65536；那么，第二次Append Object需要指定position为65536。每次操作成功后，响应头部x-oss-next-append-position也会标明下一次追加的position。
-   如果position的值和当前Object的长度不一致，OSS会返回409错误，错误码为PositionNotEqualToLength。发生上述错误时，用户可以通过响应头部x-oss-next-append-position来得到下一次position，并再次进行请求。
-   当Position值为0时，如果没有同名Appendable Object，或者同名Appendable Object长度为0，该请求成功；其他情况均视为Position和Object长度不匹配的情形。
-   当Position值为0，且没有同名Object存在，那么Append Object可以和Put Object请求一样，设置诸如x-oss-server-side-encryption之类的请求Header。这一点和Initiate Multipart Upload是一样的。如果在Position为0的请求时，加入了正确的x-oss-server-side-encryption头，那么后续的Append Object响应头部也会包含x-oss-server-side-encryption头，其值表明加密算法。后续如果需要更改meta，可以使用Copy Object请求。
-   由于并发的关系，即使用户把position的值设为了x-oss-next-append-position，该请求依然可能因为PositionNotEqualToLength而失败。
-   Append Object产生的Object长度限制和Put Object一样。
-   每次Append Object都会更新该Object的最后修改时间。
-   在position值正确的情况下，对已存在的Appendable Object追加一个长度为0的内容，该操作不会改变Object的状态。

## CRC64的计算方式 {#section_xb5_xpw_bz .section}

Appendable Object的CRC采用[ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm)标准，和XZ的计算方式一样。用Boost CRC模块的方式来定义则有：

```
typedef boost::crc_optimal<64, 0x42F0E1EBA9EA3693ULL, 0xffffffffffffffffULL, 0xffffffffffffffffULL, true, true> boost_ecma;

uint64_t do_boost_crc(const char* buffer, int length)
{
    boost_ecma crc;
    crc.process_bytes(buffer, length);
    return crc.checksum();
}
```

或是用Python crcmod的方式为：

```
do_crc64 = crcmod.mkCrcFun(0x142F0E1EBA9EA3693L, initCrc=0L, xorOut=0xffffffffffffffffL, rev=True)

print do_crc64(“123456789”)
```

## 示例 {#section_phh_cqw_bz .section}

**请求示例：**

```
POST /oss.jpg?append&position=0 HTTP/1.1
Host: oss-example.oss.aliyuncs.com
Cache-control: no-cache
Expires: Wed, 08 Jul 2015 16:57:01 GMT
Content-Encoding: utf-8
Content-Disposition: attachment;filename=oss_download.jpg
Date: Wed, 08 Jul 2015 06:57:01 GMT
Content-Type: image/jpg
Content-Length: 1717
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk=

[1717 bytes of object data]

```

**返回示例：**

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

