# GetBucketInfo {#reference_rwk_bwv_tdb .reference}

GetBucketInfo接口用于查看bucket的相关信息。

可查看内容包含如下：

-   创建时间
-   外网访问Endpoint
-   内网访问Endpoint
-   bucket的拥有者信息
-   bucket的ACL（AccessControlList）

## 请求语法 {#section_qw4_lfw_bz .section}

```
GET /?bucketInfo HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素\(Response Elements\) {#section_bkn_mfw_bz .section}

|名称|类型|描述|
|:-|:-|:-|
|BucketInfo|容器|保存Bucket信息内容的容器 子节点：Bucket节点

父节点：无

|
|Bucket|容器|保存Bucket具体信息的容器 父节点：BucketInfo节点

|
|CreationDate|时间|Bucket创建时间。时间格式 2013-07-31T10:56:21.000Z 父节点：BucketInfo.Bucket

|
|ExtranetEndpoint|字符串|Bucket访问的外网域名 父节点：BucketInfo.Bucket

|
|IntranetEndpoint|字符串|同区域ECS访问Bucket的内网域名父节点：BucketInfo.Bucket

|
|Location|字符串|Bucket所在数据中心的区域 父节点：BucketInfo.Bucket

|
|Name|字符串|Bucket名字父节点：BucketInfo.Bucket

|
|Owner|容器|用于存放Bucket拥有者信息的容器。父节点：BucketInfo.Bucket

|
|ID|字符串|Bucket拥有者的用户ID。父节点：BucketInfo.Bucket.Owner

|
|DisplayName|字符串|Bucket拥有者的名称 \(目前和ID一致\)。父节点：BucketInfo.Bucket.Owner

|
|AccessControlList|容器|存储ACL信息的容器父节点：BucketInfo.Bucket

|
|Grant|枚举字符串|Bucket的ACL权限。有效值：private、public-read、public-read-write

父节点：BucketInfo.Bucket.AccessControlList

|
|DataRedundancyType |枚举字符串|数据容灾类型有效值：LRS、ZRS

父节点：BucketInfo.Bucket

|

## 细节分析 {#section_cgv_sfw_bz .section}

-   如果Bucket不存在，返回404错误。错误码：NoSuchBucket。
-   只有Bucket的拥有者才能查看Bucket的信息，否则返回403 Forbidden错误，错误码：AccessDenied。
-   请求可以从任何一个OSS的Endpoint发起。

## 示例 {#section_i2n_tfw_bz .section}

**请求示例：**

```
Get /?bucketInfo HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Sat, 12 Sep 2015 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=

```

**成功获取Bucket信息的返回示例：**

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

**获取不存在的Bucket信息的返回示例：**

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

**获取没有权限访问的Bucket信息的返回示例：**

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

