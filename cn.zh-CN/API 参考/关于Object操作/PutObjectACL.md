# PutObjectACL {#reference_fs3_gfw_wdb .reference}

PutObjectACL接口用于修改文件（Object）的访问权限（ACL）。此操作只有Bucket Owner有权限执行，且需对Object有读写权限。

**说明：** 

-   Object ACL优先级高于Bucket ACL。例如Bucket ACL是private的，而Object ACL是public-read-write的，则所有用户都拥有这个Object的访问权限，即使这个Bucket是private bucket。如果某个Object从来没设置过ACL，则访问权限遵循Bucket ACL。
-   Object的读操作包括：GetObject、HeadObject、CopyObject和UploadPartCopy中的对源Object的读；Object的写操作包括：PutObject、PostObject、AppendObject、DeleteObject、DeleteMultipleObjects、CompleteMultipartUpload以及CopyObject对新Object的写。
-   您还可以在Object的写操作时，在请求头中带上x-oss-object-acl来设置Object ACL，效果与PutObjectA ACL等同。例如PutObject时在请求头中带上x-oss-object-acl可以在上传一个Object的同时设置此Object的ACL。

## ACL说明 {#section_bg5_wfw_wdb .section}

PutObjectACL接口通过Put请求中的`x-oss-object-acl`头来设置。目前Object有以下四种访问权限。

|名称|描述|
|:-|:-|
|private|Object是私有资源。只有该Object的Owner拥有该Object的读写权限，其他用户没有权限操作该Object。|
|public-read|Object是公共读资源。Object Owner拥有该Object的读写权限。非Object Owner只有该Object的读权限。|
|public-read-write|Object是公共读写资源。所有用户拥有对该Object的读写权限。|
|default|Object遵循其所在Bucket的读写权限，即Bucket是什么权限，Object就是什么权限。|

## 请求语法 {#section_ign_5fw_wdb .section}

```
PUT /ObjectName?acl HTTP/1.1
x-oss-object-acl: Permission
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例 {#section_awy_3gw_wdb .section}

**请求示例**

```
PUT /test-object?acl HTTP/1.1
x-oss-object-acl: public-read
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Apr 2015 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Apr 2015 05:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/管理文件/管理文件访问权限.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/管理文件/管理文件访问权限.md)
-   [PHP](../../../../../cn.zh-CN/SDK 参考/PHP/管理文件/管理文件访问权限.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/管理文件/管理文件访问权限.md)
-   [C](../../../../../cn.zh-CN/SDK 参考/C/管理文件/管理文件访问权限.md)
-   [.NET](../../../../../cn.zh-CN/SDK 参考/.NET/管理文件/管理文件访问权限.md)
-   [Node.js](../../../../../cn.zh-CN/SDK 参考/Node.js/访问权限.md)
-   [Browser.js](../../../../../cn.zh-CN/SDK 参考/Browser.js/设置访问权限.md)
-   [Ruby](../../../../../cn.zh-CN/SDK 参考/Ruby/设置访问权限.md)

## 错误码 {#section_nfp_nfc_5gb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|AccessDenied|403|接口使用者不是Bucket Owner，或Bucket Owner对Object没有读写权限。|
|InvalidArgument|400|指定的x-oss-object-acl值无效。|

