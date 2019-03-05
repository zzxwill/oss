# GetBucketInfo {#reference_rwk_bwv_tdb .reference}

GetBucketInfo接口用于查看存储空间（Bucket）的相关信息。只有Bucket的拥有者才能查看Bucket的信息。

**说明：** 请求可以从任何一个OSS的Endpoint发起。

## 请求语法 {#section_qw4_lfw_bz .section}

```
GET /?bucketInfo HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素 {#section_bkn_mfw_bz .section}

|名称|类型|描述|
|:-|:-|:-|
|BucketInfo|容器| 保存Bucket信息内容的容器

 子节点：Bucket节点

 父节点：无

 |
|Bucket|容器| 保存Bucket具体信息的容器

 父节点：BucketInfo 节点

 |
|CreationDate|时间| Bucket创建时间

 时间格式：2013-07-31T10:56:21.000Z

 父节点：BucketInfo.Bucket

 |
|ExtranetEndpoint|字符串| Bucket的外网域名

 父节点：BucketInfo.Bucket

 |
|IntranetEndpoint|字符串| 同区域ECS访问Bucket的内网域名

 父节点：BucketInfo.Bucket

 |
|Location|字符串| Bucket的地域

 父节点：BucketInfo.Bucket

 |
|Name|字符串| Bucket名称

 父节点：BucketInfo.Bucket

 |
|Owner|容器| 用于存放Bucket拥有者信息的容器

 父节点：BucketInfo.Bucket

 |
|ID|字符串| Bucket拥有者的用户ID

 父节点：BucketInfo.Bucket.Owner

 |
|DisplayName|字符串| Bucket拥有者的名称 \(目前和用户ID一致\)

 父节点：BucketInfo.Bucket.Owner

 |
|AccessControlList|容器| 存储ACL信息的容器

 父节点：BucketInfo.Bucket

 |
|Grant|枚举字符串| Bucket的ACL权限

 有效值：private、public-read、public-read-write

 父节点：BucketInfo.Bucket.AccessControlList

 |
|DataRedundancyType |枚举字符串| 数据容灾类型

 有效值：LRS、ZRS

 父节点：BucketInfo.Bucket

 |
|StorageClass|字符串| Bucket存储类型

 有效值：Standard、IA、Archive

 |

## 示例 {#section_i2n_tfw_bz .section}

**请求示例**

```
Get /?bucketInfo HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Sat, 12 Sep 2015 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=

```

**返回示例（成功获取Bucket信息）**

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Sat, 12 Sep 2015 07:51:28 GMT
Connection: keep-alive
Content-Length: 531  
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<BucketInfo>
  <Bucket>
    <CreationDate>2013-07-31T10:56:21.000Z</CreationDate>
    <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
    <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
    <Location>oss-cn-hangzhou</Location>
    <Name>oss-example</Name>
    <Owner>
      <DisplayName>username</DisplayName>
      <ID>271834739143143</ID>
    </Owner>
    <AccessControlList>
      <Grant>private</Grant>
    </AccessControlList>
  </Bucket>
</BucketInfo>
```

**返回示例（获取不存在的Bucket信息）**

```
HTTP/1.1 404 
x-oss-request-id: 534B371674E88A4D8906009B
Date: Sat, 12 Sep 2015 07:51:28 GMT
Connection: keep-alive
Content-Length: 308  
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>NoSuchBucket</Code>
  <Message>The specified bucket does not exist.</Message>
  <RequestId>568D547F31243C673BA14274</RequestId>
  <HostId>nosuchbucket.oss.aliyuncs.com</HostId>
  <BucketName>nosuchbucket</BucketName>
</Error>
```

**返回示例（获取没有权限访问的Bucket信息）**

```
HTTP/1.1 403
x-oss-request-id: 534B371674E88A4D8906008C
Date: Sat, 12 Sep 2015 07:51:28 GMT
Connection: keep-alive
Content-Length: 209  
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>AccessDenied</Code>
  <Message>AccessDenied</Message>
  <RequestId>568D5566F2D0F89F5C0EB66E</RequestId>
  <HostId>test.oss.aliyuncs.com</HostId>
</Error>
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../intl.zh-CN/SDK 参考/Java/存储空间.md)
-   [Python](../../../../../intl.zh-CN/SDK 参考/Python/存储空间.md)
-   [PHP](../../../../../intl.zh-CN/SDK 参考/PHP/存储空间.md)
-   [Go](../../../../../intl.zh-CN/SDK 参考/Go/存储空间.md)
-   [C](../../../../../intl.zh-CN/SDK 参考/C/存储空间.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|AccessDenied|403|没有查看该Bucket信息的权限。只有Bucket的拥有者才能查看Bucket的信息。|

