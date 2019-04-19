# PutBucketLifecycle {#reference_xlw_dbv_tdb .reference}

PutBucketLifecycle接口用于设置存储空间（Bucket）的生命周期规则。生命周期规则开启后，OSS将按照配置规则，定期自动删除与规则相匹配的文件（Object）。只有Bucket的拥有者才能发起此请求。

**说明：** 

-   如果Bucket此前没有设置过生命周期规则，此操作会创建一个新的生命周期规则；如果Bucket此前设置过生命周期规则，此操作会覆写先前的配置。
-   此操作可以对Object以及Part（以分片方式上传，但最后未提交的分片）设置过期时间。

## 请求语法 {#section_bfn_rcw_bz .section}

```
PUT /?lifecycle HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Authorization: SignatureValue 
Host: BucketName.oss.aliyuncs.com
<?xml version="1.0" encoding="UTF-8"?>
<LifecycleConfiguration>
  <Rule>
    <ID>RuleID</ID>
    <Prefix>Prefix</Prefix>
    <Status>Status</Status>
    <Expiration>
      <Days>Days</Days>
    </Expiration>
    <Transition>
      <Days>Days</Days>
      <StorageClass>StorageClass</StorageClass>
    </Transition>
    <AbortMultipartUpload>
      <Days>Days</Days>
    </AbortMultipartUpload>
  </Rule>
</LifecycleConfiguration>
```

## 请求元素 {#section_vrs_vcw_bz .section}

|名称|类型|是否必选|描述|
|--|--|----|--|
|CreatedBeforeDate|字符串|Days和CreatedBeforeDate二选一| 指定一个日期，OSS会对该日期之前的数据执行生命周期规则。日期必需服从ISO8601的格式，并总是UTC的零点。 例如：2002-10-11T00:00:00.000Z，表示将最后更新时间早于2002-10-11T00:00:00.000Z 的Object删除或转换成其他存储类型，等于或晚于这个时间的Object不会被删除或转储。

 父节点：Expiration或者AbortMultipartUpload

 |
|Days|正整数|Days和CreatedBeforeDate二选一| 指定生命周期规则在Object最后更新过后多少天生效。

 父节点：Expiration

 |
|Expiration|容器|否| 指定Object生命周期规则的过期属性。

 子节点：Days或CreatedBeforeDate

 父节点：Rule

 |
|AbortMultipartUpload|容器|否| 指定未完成分片上传的过期属性。

 子节点：Days或CreatedBeforeDate

 父节点：Rule

 |
|ID|字符串|否| 规则唯一的ID。最多由255个字节组成。如没有指定，或者该值为空时，OSS会自动生成一个唯一ID。

 子节点：无

 父节点：Rule

 |
|LifecycleConfiguration|容器|是| Lifecycle配置的容器，最多可容纳1000条规则。

 子节点：Rule

 父节点：无

 |
|Prefix|字符串|是| 指定规则所适用的前缀。只有匹配前缀的Object才可能被该规则所影响。前缀不可重叠。

 子节点：无

 父节点：Rule

 |
|Rule|容器|是| 表述一条规则。

 **说明：** 

-   不支持Archvie Bucket创建转储规则。
-   Object设置过期时间必须大于转储为IA或者Archive存储类型的时间。

 子节点：ID，Prefix，Status，Expiration

 父节点：LifecycleConfiguration

 |
|Status|字符串|是| 如果值为Enabled，那么OSS会定期执行该规则；如果为Disabled，那么OSS会忽略该规则。

 父节点：Rule

 有效值：Enabled，Disabled

 |
|StorageClass|字符串|如果父节点Transition已设置，则为必选| 指定Object转储的存储类型。

 **说明：** IA Bucket中Object可以转储为Archive存储类型，但不支持转储为Standard存储类型。

 取值：IA，Archive

 父节点：Transition

 |
|Transition|容器|否| 指定Object在有效生命周期中，OSS何时将Object转储为IA或者Archive存储类型 。

 **说明：** Standard Bucket中的Standard Object可以转储为IA、Archive存储类型，但转储Archive存储类型的时间必须比转储IA存储类型的时间长。例如Transition IA设置Days为30，Transition Archive设置Days必须大于IA。

 |
|Tag|容器|否|指定规则所适用的对象标签，可设置多个。 父节点：Rule

 子节点：Key， Value

 |
|Key|字符串|若父节点Tag已设置，则为必需|Tag Key 父节点：Tag

 |
|Value|字符串|若父节点Tag已设置，则为必需|Tag Value 父节点：Tag

 |

## 示例 {#section_ox3_zcw_bz .section}

 **请求示例** 

```
PUT /?lifecycle HTTP/1.1
Host: oss-example.oss.aliyuncs.com
Content-Length: 443
Date: Thu , 8 Jun 2017 13:08:38 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:PYbzsdWAIWAlMW8luk*****
<?xml version="1.0" encoding="UTF-8"?>
<LifecycleConfiguration>
  <Rule>
    <ID>delete objects and parts after one day</ID>
    <Prefix>logs/</Prefix>
    <Status>Enabled</Status>
    <Expiration>
      <Days>1</Days>
    </Expiration>
    <AbortMultipartUpload>
      <Days>1</Days>
    </AbortMultipartUpload>
  </Rule>
  <Rule>
    <ID>transit objects to IA after 30, to Archive 60, expire after 10 years</ID>
    <Prefix>data/</Prefix>
    <Status>Enabled</Status>
    <Transition>
      <Days>30</Days>
      <StorageClass>IA</StorageClass>
    </Transition>
    <Transition>
      <Days>60</Days>
      <StorageClass>Archive</StorageClass>
    </Transition>
    <Expiration>
      <Days>3600</Days>
    </Expiration>
  </Rule>
  <Rule>
    <ID>transit objects to Archive after 60 days</ID>
    <Prefix>important/</Prefix>
    <Status>Enabled</Status>
    <Transition>
      <Days>6</Days>
      <StorageClass>Archive</StorageClass>
    </Transition>
  </Rule>
  <Rule>
    <ID>delete created before date</ID>
    <Prefix>backup/</Prefix>
    <Status>Enabled</Status>
    <Expiration>
      <CreatedBeforeDate>2017-01-01T00:00:00.000Z</CreatedBeforeDate>
    </Expiration>
    <AbortMultipartUpload>
      <CreatedBeforeDate>2017-01-01T00:00:00.000Z</CreatedBeforeDate>
    </AbortMultipartUpload>
  </Rule>
  <Rule>
    <ID>r1</ID>
    <Prefix>rule1</Prefix>
    <Tag><Key>xx</Key><Value>1</Value></Tag>
    <Tag><Key>yy</Key><Value>2</Value></Tag>
    <Status>Enabled</Status>
    <Expiration>
      <Days>30</Days>
    </Expiration>
  </Rule>
  <Rule>
    <ID>r2</ID>
    <Prefix>rule2</Prefix>
    <Tag><Key>xx</Key><Value>1</Value></Tag>
    <Status>Enabled</Status>
    <Transition>
      <Days>60</Days>
    <StorageClass>Archive</StorageClass>
    </Transition>
  </Rule>
</LifecycleConfiguration>
			
```

 **返回示例** 

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674A4D890*****
Date: Thu , 8 Jun 2017 13:08:38 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../intl.zh-CN/SDK 参考/Java/生命周期.md)
-   [Python](../../../../intl.zh-CN/SDK 参考/Python/生命周期.md)
-   [PHP](../../../../intl.zh-CN/SDK 参考/PHP/生命周期.md)
-   [Go](../../../../intl.zh-CN/SDK 参考/Go/生命周期.md)
-   [C](../../../../intl.zh-CN/SDK 参考/C/生命周期.md)
-   [.NET](../../../../intl.zh-CN/SDK 参考/.NET/生命周期.md)
-   [Node.js](../../../../intl.zh-CN/SDK 参考/Node.js/生命周期.md)
-   [Ruby](../../../../intl.zh-CN/SDK 参考/Ruby/生命周期.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|AccessDenied|403|没有操作权限。只有Bucket的拥有者才能发起PutBucketLifecycle请求。|
|InvalidArgument|400| -   OSS支持Standard Bucket中Standard Objects转储为IA、Archive存储类型。Standard Bucket可以针对一个Object同时配置转储IA和Archive存储类型规则。转储Archive存储类型的时间必须比转储IA存储类型的时间长。
-   Object的指定过期时间必须比转储为IA或者Archive存储类型的时间长。

 |

