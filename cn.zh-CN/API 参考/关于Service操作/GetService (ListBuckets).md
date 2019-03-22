# GetService \(ListBuckets\) {#reference_ahf_k4t_tdb .reference}

对于服务地址做Get请求可以返回请求者拥有的所有Bucket，其中“/”表示根目录。

## 请求语法 {#section_glm_xkr_bz .section}

```
GET / HTTP/1.1
Host: oss.example.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求参数 {#section_z4x_zkr_bz .section}

GetService\(ListBucket\)时，可以通过prefix、marker和max-keys对list做限定，返回部分结果。

|名称|类型|是否必需|描述|
|--|--|----|--|
|prefix|字符串|否|限定返回的bucket name必须以prefix作为前缀，可以不设定，不设定时不过滤前缀信息。默认值：无

|
|marker|字符串|否|设定结果从marker之后按字母排序的第一个开始返回，可以不设定，不设定时从头开始返回数据。。默认值：无

|
|max-keys|字符串|否|限定此次返回bucket的最大数，如果不设定，默认为100，max-keys取值不能大于1000默认值：100

|

## 响应元素 {#section_fvy_clr_bz .section}

|名称|类型|描述|
|--|--|--|
|ListAllMyBucketsResult|容器|保存Get Service请求结果的容器。子节点：Owner、Buckets

父节点：None

|
|Prefix|字符串|本次查询结果的前缀，当bucket未全部返回时才有此节点。父节点：ListAllMyBucketsResult

|
|Marker|字符串|标明这次GetService\(ListBucket\)的起点，当bucket未全部返回时才有此节点。父节点：ListAllMyBucketsResult

|
|MaxKeys|字符串|响应请求内返回结果的最大数目，当bucket未全部返回时才有此节点。父节点：ListAllMyBucketsResult

|
|IsTruncated|枚举字符串|指明是否所有的结果都已经返回：“true”表示本次没有返回全部结果；“false”表示本次已经返回了全部结果。当bucket未全部返回时才有此节点。有效值：true、false

父节点：ListAllMyBucketsResult

|
|NextMarker|字符串|表示下一次GetService\(ListBucket\)可以以此为marker，将未返回的结果返回。当bucket未全部返回时才有此节点。父节点：ListAllMyBucketsResult

|
|Owner|容器|用于存放Bucket拥有者信息的容器。父节点：ListAllMyBucketsResult

|
|ID|字符串|Bucket拥有者的用户ID。父节点：ListAllMyBucketsResult.Owner

|
|DisplayName|字符串|Bucket拥有者的名称 \(目前和ID一致\)。父节点：ListAllMyBucketsResult.Owner

|
|Buckets|容器|保存多个Bucket信息的容器。子节点：Bucket

父节点：ListAllMyBucketsResult

|
|Bucket|容器|保存bucket信息的容器。子节点：Name, CreationDate, Location

父节点：ListAllMyBucketsResult.Buckets

|
|Name|字符串|Bucket名称。父节点：ListAllMyBucketsResult.Buckets.Bucket

|
|CreateDate|时间 \(格式：yyyy-mm-ddThh:mm:ss.timezone, e.g., 2011-12-01T12:27:13.000Z\)|Bucket创建时间父节点：ListAllMyBucketsResult.Buckets.Bucket

|
|Location|字符串|Bucket所在的数据中心。父节点：ListAllMyBucketsResult.Buckets.Bucket

|
|ExtranetEndpoint|字符串|Bucket访问的外网域名。 父节点：ListAllMyBucketsResult.Buckets.Bucket

|
|IntranetEndpoint|字符串|同区域ECS访问Bucket的内网域名。父节点：ListAllMyBucketsResult.Buckets.Bucket

|
|StorageClass|字符串|Bucket存储类型，支持“Standard”、“IA”、“Archive”（目前只有部分区域支持“Archive”类型）。父节点：ListAllMyBucketsResult.Buckets.Bucket

|

## 细节分析 {#section_ets_qlr_bz .section}

-   GetService这个API只对验证通过的用户有效。
-   如果请求中没有用户验证信息（即匿名访问），返回403 Forbidden。错误码：AccessDenied。
-   当所有的bucket都返回时，返回的xml中不包含Prefix、Marker、MaxKeys、IsTruncated、NextMarker节点，如果还有部分结果未返回，则增加上述节点，其中NextMarker用于继续查询时给marker赋值。

## 示例 {#section_tm3_slr_bz .section}

**请求示例1**

```
GET / HTTP/1.1
Date: Thu, 15 May 2014 11:18:32 GMT
Host: oss-cn-hangzhou.aliyuncs.com
Authorization: OSS nxj7dtl******hcyl5hpvnhi:COS3OQkfQPnKmYZTEHYv2qUl5jI=
```

**返回示例1**

```
HTTP/1.1 200 OK
Date: Thu, 15 May 2014 11:18:32 GMT
Content-Type: application/xml
Content-Length: 556
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D74
<?xml version="1.0" encoding="UTF-8"?>
<ListAllMyBucketsResult>
  <Owner>
    <ID>51264</ID>
    <DisplayName>51264</DisplayName>
  </Owner>
  <Buckets>
    <Bucket>
      <CreationDate>2015-12-17T18:12:43.000Z</CreationDate>
      <ExtranetEndpoint>oss-cn-shanghai.aliyuncs.com</ExtranetEndpoint>
      <IntranetEndpoint>oss-cn-shanghai-internal.aliyuncs.com</IntranetEndpoint>
      <Location>oss-cn-shanghai</Location>
      <Name>app-base-oss</Name>
      <StorageClass>Standard</StorageClass>
    </Bucket>
    <Bucket>
      <CreationDate>2014-12-25T11:21:04.000Z</CreationDate>
      <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
      <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
      <Location>oss-cn-hangzhou</Location>
      <Name>atestleo23</Name>
      <StorageClass>IA</StorageClass>
    </Bucket>
  </Buckets>
</ListAllMyBucketsResult>
```

**请求示例2**

```
GET /?prefix=xz02tphky6fjfiuc&max-keys=1 HTTP/1.1
Date: Thu, 15 May 2014 11:18:32 GMT
Host: oss-cn-hangzhou.aliyuncs.com
Authorization: OSS nxj7dt******whcyl5hpvnhi:COS3OQkfQPnKmYZTEHYv2qUl5jI=
```

**返回示例2**

```
HTTP/1.1 200 OK
Date: Thu, 15 May 2014 11:18:32 GMT
Content-Type: application/xml
Content-Length: 545
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D75
<?xml version="1.0" encoding="UTF-8"?>
<ListAllMyBucketsResult>
  <Prefix>xz02tphky6fjfiuc</Prefix>
  <Marker></Marker>
  <MaxKeys>1</MaxKeys>
  <IsTruncated>true</IsTruncated>
  <NextMarker>xz02tphky6fjfiuc0</NextMarker>
  <Owner>
    <ID>ut_test_put_bucket</ID>
    <DisplayName>ut_test_put_bucket</DisplayName>
  </Owner>
  <Buckets>
    <Bucket>
      <CreationDate>2014-05-15T11:18:32.000Z</CreationDate>
      <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
      <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
      <Location>oss-cn-hangzhou</Location>
      <Name>xz02tphky6fjfiuc0</Name>
      <StorageClass>Standard</StorageClass>
    </Bucket>
  </Buckets>
</ListAllMyBucketsResult>
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../intl.zh-CN/SDK 参考/Java/存储空间.md#section_vsh_fjf_2gb)
-   [Python](../../../../../intl.zh-CN/SDK 参考/Python/存储空间.md#)
-   [PHP](../../../../../intl.zh-CN/SDK 参考/PHP/存储空间.md#)
-   [Go](../../../../../intl.zh-CN/SDK 参考/Go/存储空间.md#ul_ofr_nbx_kfb)
-   [C](../../../../../intl.zh-CN/SDK 参考/C/存储空间.md#)
-   [.NET](../../../../../intl.zh-CN/SDK 参考/.NET/存储空间.md#)
-   [iOS](../../../../../intl.zh-CN/SDK 参考/iOS/存储空间.md#ul_xpj_skk_zgb)
-   [Node.js](../../../../../intl.zh-CN/SDK 参考/Node.js/存储空间.md#section_l4m_ppk_lfb)
-   [Ruby](../../../../../intl.zh-CN/SDK 参考/Ruby/存储空间.md#)

