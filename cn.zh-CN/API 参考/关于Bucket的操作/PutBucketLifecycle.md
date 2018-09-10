# PutBucketLifecycle {#reference_xlw_dbv_tdb .reference}

Bucket的拥有者可以通过PutBucketLifecycle接口来设置Bucket的Lifecycle。Lifecycle开启后，OSS将按照配置，定期自动删除与Lifecycle规则相匹配的Object。

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

## 请求元素\(Request Elements\) {#section_vrs_vcw_bz .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|CreatedBeforeDate|字符串|Days和CreatedBeforeDate二选一|指定哪天之前的数据被执行Lifecycle规则。日期必需服从ISO8601的格式，并且总是UTC的零点。 例如：2002-10-11T00:00:00.000Z，表示将最后更新时间早于 2002-10-11T00:00:00.000Z 的Object删除或转换成其他存储类型，晚于这个时间的Object不会被删除或转储。父节点：Expiration或者AbortMultipartUpload

|
|Days|正整数|Days和CreatedBeforeDate二选一|指定规则在对象最后更新时间过后多少天生效。 父节点：Expiration

|
|Expiration|容器|否|指定Object规则的过期属性。 子节点：Days或CreatedBeforeDate

父节点：Rule

|
|AbortMultipartUpload|容器|否|指定未完成的Part规则的过期属性。 子节点：Days或CreatedBeforeDate

父节点：Rule

|
|ID|字符串|否|规则唯一的ID。最多由255字节组成。当用户没有指定，或者该值为空时，OSS会为用户生成一个唯一值。 子节点：无

父节点：Rule

|
|LifecycleConfiguration|容器|是|Lifecycle配置的容器，最多可容纳1000条规则。 子节点：Rule

父节点：无

|
|Prefix|字符串|是|指定规则所适用的前缀。只有匹配前缀的对象才可能被该规则所影响。不可重叠。 子节点：无

父节点：Rule

|
|Rule|容器|是|表述一条规则 子节点：ID，Prefix，Status，Expiration

父节点：LifecycleConfiguration

|
|Status|字符串|是|如果其值为Enabled，那么OSS会定期执行该规则；如果是Disabled，那么OSS会忽略该规则。 父节点：Rule

有效值：Enabled，Disabled

|
|StorageClass|字符串|如果父节点Transition已设置，则为必需|指定对象转储到OSS的目标存储类型。取值：-   IA
-   Archive

父节点：Transition

 |
|Transition|容器|否|指定Object在有效生命周期中，OSS何时将对象转储到IA或者Archive存储类型 。|

## 细节分析 {#section_udl_ycw_bz .section}

-   只有Bucket的拥有者才能发起Put Bucket Lifecycle请求，否则返回403 Forbidden消息。错误码：AccessDenied。
-   如果此前没有设置过Lifecycle，此操作会创建一个新的Lifecycle配置；否则，就覆写先前的配置。
-   可以对Object设置过期时间，也可以对Part设置过期时间。这里的Part指的是以分片上传方式上传，但最后未提交的分片。

转储注意事项：

-   支持Standard Bucket中Standard Objects转储为IA、Archive存储类型，Standard Bucket可以针对一个Object同时配置转储IA和Archive存储类型规则，同时配置转储IA和Archive类型情况下，转储Archive的时间必须比转储IA的时间长。例如Transition IA设置Days为30，Transition Archive设置Days必须大于IA。否则，报InvalidArgument错。
-   Object设置过期时间必须大于转为IA或者Archive的时间。否则，报InvalidArgument错。
-   支持IA Bucket中Objects转储为Archive存储类型。
-   不支持Archvie Bucket创建转储规则。
-   不支持IA类型Object转储为Standard。
-   不支持Archive类型Object转储为IA或Standard。

## 示例 {#section_ox3_zcw_bz .section}

**请求示例：**

```
PUT /?lifecycle HTTP/1.1
Host: oss-example.oss.aliyuncs.com
Content-Length: 443
Date: Thu , 8 Jun 2017 13:08:38 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:PYbzsdWSMrAIWAlMW8luWekJ8=
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
</LifecycleConfiguration>
```

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu , 8 Jun 2017 13:08:38 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

