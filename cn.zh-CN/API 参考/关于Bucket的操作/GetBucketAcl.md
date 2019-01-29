# GetBucketAcl {#reference_hgp_psv_tdb .reference}

GetBucketAcl用来获取某个Bucket的访问权限。

**说明：** 只有Bucket的拥有者才能使用GetBucketACL这个接口。

## 请求语法 {#section_d1r_t2w_bz .section}

```
GET /?acl HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素\(Response Elements\) {#section_iwr_52w_bz .section}

|名称|类型|描述|
|--|--|--|
|AccessControlList|容器|存储ACL信息的容器类。父节点：AccessControlPolicy

|
|AccessControlPolicy|容器|保存GetBucketACL结果的容器。父节点：None

|
|DisplayName|字符串|Bucket拥有者的名称（目前和ID一致）。父节点：AccessControlPolicy.Owner

|
|Grant|枚举字符串|Bucket的ACL权限。有效值：private、public-read、public-read-write

父节点：AccessControlPolicy.AccessControlList

|
|ID|字符串|Bucket拥有者的用户ID。父节点：AccessControlPolicy.Owner

|
|Owner|容器|保存Bucket拥有者信息的容器。父节点：AccessControlPolicy

|

## 示例 {#section_gwr_x2w_bz .section}

请求示例：

```
GET /?acl HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 04:11:23 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0FmgbrQ0=

```

返回示例：

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 04:11:23 GMT 
Content-Length: 253
Content-Type: application/xml
Connection: keep-alive
Server: AliyunOSS

<?xml version="1.0" ?>
<AccessControlPolicy>
    <Owner>
        <ID>00220120222</ID>
        <DisplayName>user_example</DisplayName>
    </Owner>
    <AccessControlList>
        <Grant>public-read</Grant>
    </AccessControlList>
</AccessControlPolicy>
```

