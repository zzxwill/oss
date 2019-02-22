# GetObjectMeta {#reference_sg4_k2w_wdb .reference}

GetObjectMeta接口用于获取一个文件（Object）的元数据信息，包括该Object的ETag、Size、LastModified信息，并且不返回该Object的内容。

**说明：** 

-   如果Object类型为符号链接，会返回符号链接自身信息。
-   无论正常返回还是非正常返回，GetObjectMeta均不返回消息体。

## 请求语法 {#section_nvn_42w_wdb .section}

```
HEAD /ObjectName?objectMeta HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应头 {#section_und_k4s_qgb .section}

|响应头|类型|描述|
|:--|--|:-|
|Content-Length|字符串| Object的文件大小。

 |
|ETag|字符串| Object生成时会创建ETag \(entity tag\)，ETag用于标示一个Object的内容。

 对于PutObject请求创建的Object，ETag值是其内容的MD5值；对于其他方式创建的Object，ETag值是其内容的UUID。ETag值可以用于检查Object内容是否发生变化。不建议用户使用ETag来作为Object内容的MD5校验数据完整性。

 默认值：无

 |

## 示例 {#section_wkh_52w_wdb .section}

**请求示例**

```
HEAD /oss.jpg?objectMeta HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Apr 2015 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0FmgbrQ0=
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Apr 2015 05:21:12 GMT
ETag: "5B3C1A2E053D763E1B002CC607C5A0FE"
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
Content-Length: 344606
Connection: keep-alive
Server: AliyunOSS
```

## SDK {#section_pfh_pnw_tgb .section}

GetObjectMeta接口所对应的各语言SDK如下：

-   [Java](../../../../../intl.zh-CN/SDK 参考/Java/管理文件/管理文件元信息.md)
-   [Python](../../../../../intl.zh-CN/SDK 参考/Python/管理文件/管理文件元信息.md)
-   [Java](../../../../../intl.zh-CN/SDK 参考/Java/管理文件/管理文件元信息.md)
-   [Python](../../../../../intl.zh-CN/SDK 参考/Python/管理文件/管理文件元信息.md)
-   [PHP](../../../../../intl.zh-CN/SDK 参考/PHP/管理文件/管理文件元信息.md)
-   [Go](../../../../../intl.zh-CN/SDK 参考/Go/管理文件/管理文件访问权限.md)
-   [C](../../../../../intl.zh-CN/SDK 参考/C/管理文件/管理文件访问权限.md)
-   [.NET](../../../../../intl.zh-CN/SDK 参考/.NET/管理文件/管理文件元信息.md)
-   [iOS](../../../../../intl.zh-CN/SDK 参考/iOS/管理文件.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|Not Found|404|目标Object不存在。|

