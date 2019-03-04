# GetObjectACL {#reference_lzc_24w_wdb .reference}

GetObjectACL接口用来获取某个Bucket下的某个Object的访问权限（ACL）。

**说明：** 如果一个Object从未设置过ACL，则调用GetObjectACL时，返回的ObjectACL为default，表示该Object的ACL遵循Bucket ACL。即如果Bucket的访问权限是private，则该object的访问权限也是private。

## 请求语法 {#section_pzf_h4w_wdb .section}

```
GET /ObjectName?acl HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素 {#section_qrd_j4w_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|AccessControlList|容器| 存储ACL信息的容器

 父节点：AccessControlPolicy

 |
|AccessControlPolicy|容器| 保存Get Object ACL结果的容器

 父节点：None

 |
|DisplayName|字符串| Bucket拥有者的名称\(目前和ID一致\)

 父节点：AccessControlPolicy.Owner

 |
|Grant|枚举字符串| Object的ACL权限

 有效值：private，public-read，public-read-write

 父节点：AccessControlPolicy.AccessControlList

 |
|ID|字符串| Bucket拥有者的用户ID

 父节点：AccessControlPolicy.Owner

 |
|Owner|容器| 保存Bucket拥有者信息的容器

 父节点：AccessControlPolicy

 |

## 示例 {#section_swy_3pw_wdb .section}

**请求示例**

```
GET /test-object?acl HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Apr 2015 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0FmgbrQ0=
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Apr 2015 05:21:12 GMT
Content-Length: 253
Content-Tupe: application/xml
Connection: keep-alive
Server: AliyunOSS

<?xml version="1.0" ?>
<AccessControlPolicy>
    <Owner>
        <ID>00220120222</ID>
        <DisplayName>00220120222</DisplayName>
    </Owner>
    <AccessControlList>
        <Grant>public-read </Grant>
    </AccessControlList>
</AccessControlPolicy>
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   Java
-   Python
-   PHP
-   Go
-   C
-   .NET
-   Android
-   iOS
-   Node.js
-   Browser.js
-   Ruby

## 错误码 {#section_nfp_nfc_5gb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|AccessDenied|403|错误提示：You do not have read acl permission on this object。只有Bucket的拥有者才能使用GetObjectACL这个接口来获取该Bucket下某个Object的ACL，非Bucket拥有者调用该接口时出现此报错。

|

