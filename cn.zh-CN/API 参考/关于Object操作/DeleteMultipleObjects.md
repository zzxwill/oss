# DeleteMultipleObjects {#reference_ydg_25v_wdb .reference}

DeleteMultipleObjects接口用于删除同一个存储空间（Bucket）中的多个文件（Object）。

## 请求语法 {#section_iqs_fvv_wdb .section}

```
POST /?delete HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: ContentLength
Content-MD5: MD5Value
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<Delete>
  <Quiet>true</Quiet>
  <Object>
    <Key>key</Key>
  </Object>
…
</Delete>
```

## 请求头 {#section_ztg_wzw_wdb .section}

OSS会根据以下请求头验证收到的消息体，消息体正确才会执行删除操作。

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|encoding-type|字符串|否| Key使用UTF-8字符。如果Key中包含XML 1.0标准不支持的控制字符，您可以通过指定encoding-type对返回结果中的Key进行编码。

 默认值：无

 可选值：url

 |
|Content-Length|字符串|是| 用于描述HTTP消息体的传输长度。

 OSS会根据此请求头验证收到的消息体，消息体正确才会执行删除操作。

 |
|Content-MD5|字符串|是| Content-MD5是一串由MD5算法生成的值，该请求头用于检查消息内容是否与发送时一致。上传了Content-MD5请求头后，OSS会计算消息体的Content-MD5并检查一致性。

 **说明：** 将DeleteMultipleObjects的请求消息体经过MD5加密后得到一个128位字节数组。然后将该字节数组用base64算法编码，编码后得到的字符串即Content-MD5字段内容。

 |

## 请求元素 {#section_l1c_svv_wdb .section}

|名称|类型|是否必选|描述|
|:-|:-|----|:-|
|Delete|容器|是| 保存DeleteMultipleObjects请求的容器。

 子节点：一个或多个Object元素，Quiet元素

 父节点： None

 |
|Object|容器|是| 保存一个Object信息的容器。

 子节点：Key

 父节点：Delete

 |
|Key|字符串|是| 被删除Object的名字。

 父节点：Object

 |
|Quiet|枚举字符串|是| 打开简单响应模式的开关。

 DeleteMultipleObjects提供以下两种消息返回模式：

-   简单模式\(quiet\)：OSS返回的消息体中只包含删除过程中出错的Object结果。如果所有删除都成功，则没有消息体。
-   详细模式\(verbose\)：OSS返回的消息体中会包含所有删除Object的结果。默认采用详细模式。

 有效值：true（开启简单模式）、false（开启详细模式）

 默认值：false

 父节点：Delete

 |

## 响应元素 {#section_ync_yyv_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|Deleted|容器| 保存被成功删除的Object的容器。

 子节点：Key

 父节点：DeleteResult

 |
|DeleteResult|容器| 保存DeleteMultipleObjects请求结果的容器。

 子节点：Deleted

 父节点：None

 |
|Key|字符串| 被删除Object的名字。

 父节点：Deleted

 |
|EncodingType|字符串| 返回结果中编码使用的类型。如果请求的参数中指定了encoding-type，则返回的结果会对Key进行编码。

 父节点：容器

 |

## 示例 {#section_mcs_21w_wdb .section}

**关闭简单响应模式的请求示例**

```
POST /?delete HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Feb 2012 12:26:16 GMT
Content-Length:151
Content-MD5: ohhnqLBJFiKkPSBO1eNaUA==
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:+z3gBfnFAxBcBDgx27Y/jEfbfu8=
<?xml version="1.0" encoding="UTF-8"?>
<Delete> 
  <Quiet>false</Quiet>  
  <Object> 
    <Key>multipart.data</Key> 
  </Object>  
  <Object> 
    <Key>test.jpg</Key> 
  </Object>  
  <Object> 
    <Key>demo.jpg</Key> 
  </Object> 
</Delete>
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 78320852-7eee-b697-75e1-b6db0f4849e7
Date: Wed, 29 Feb 2012 12:26:16 GMT
Content-Length: 244
Content-Type: application/xml
Connection: keep-alive
Server: AliyunOSS
<?xml version="1.0" encoding="UTF-8"?>
<DeleteResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Deleted>
       <Key>multipart.data</Key>
    </Deleted>
    <Deleted>
       <Key>test.jpg</Key>
    </Deleted>
    <Deleted>
       <Key>demo.jpg</Key>
    </Deleted>
</DeleteResult>
```

**打开简单响应模式的请求示例**

```
POST /?delete HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Feb 2012 12:33:45 GMT
Content-Length:151
Content-MD5: ohhnqLBJFiKkPSBO1eNaUA==
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:WuV0Jks8RyGSNQrBca64kEExJDs=
<?xml version="1.0" encoding="UTF-8"?>
<Delete> 
  <Quiet>true</Quiet>  
  <Object> 
    <Key>multipart.data</Key> 
  </Object>  
  <Object> 
    <Key>test.jpg</Key> 
  </Object>  
  <Object> 
    <Key>demo.jpg</Key> 
  </Object> 
</Delete>
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Feb 2012 12:33:45 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK {#section_egl_m2c_5gb .section}

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/管理文件/删除文件.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/管理文件/删除文件.md)
-   [PHP](../../../../../cn.zh-CN/SDK 参考/Python/管理文件/删除文件.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/管理文件/删除文件.md)
-   [C](../../../../../cn.zh-CN/SDK 参考/C/管理文件/删除文件.md)
-   [.NET](../../../../../cn.zh-CN/SDK 参考/.NET/管理文件/删除文件.md)
-   [iOS](../../../../../cn.zh-CN/SDK 参考/iOS/管理文件.md)
-   [Node.js](../../../../../cn.zh-CN/SDK 参考/Node.js/管理文件.md)
-   [Browser.js](../../../../../cn.zh-CN/SDK 参考/Browser.js/管理文件.md)
-   [Ruby](../../../../../cn.zh-CN/SDK 参考/Ruby/管理文件.md)

## 错误码 {#section_nfp_nfc_5gb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|InvalidDigest|400|上传了Content-MD5请求头后，OSS会计算消息体的Content-MD5并检查一致性，如果不一致会返回此错误码。|
|MalformedXML|400| -   消息体最大允许2 MB的内容，超过2 MB会返回此错误码。
-   DeleteMultipleObjects接口最多支持单次请求删除1000个Object。单次请求超过1000个Object返回此错误码。

 |

