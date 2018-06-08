# DeleteMultipleObjects {#reference_ydg_25v_wdb .reference}

DeleteMultipleObjects支持用户通过一个HTTP请求删除同一个Bucket中的多个Object。

Delete Multiple Objects操作支持一次请求内最多删除1000个Object，并提供两种返回模式：详细\(verbose\)模式和简单\(quiet\)模式。

-   详细模式：OSS返回的消息体中会包含每一个删除Object的结果。
-   简单模式：OSS返回的消息体中只包含删除过程中出错的Object结果；如果所有删除都成功的话，则没有消息体。

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

## 请求参数\(Request Parameters\) {#section_wmb_lvv_wdb .section}

Delete Multiple Objects时，可以通过encoding-type对返回结果中的Key进行编码。

|名称|描述|
|:-|:-|
|encoding-type|指定对返回的Key进行编码，目前支持url编码。Key使用UTF-8字符，但xml 1.0标准不支持解析一些控制字符，比如ascii值从0到10的字符。对于Key中包含xml 1.0标准不支持的控制字符，可以通过指定encoding-type对返回的Key进行编码。数据类型：字符串

默认值：无

可选值：url

|

## 请求元素\(Request Elements\) {#section_l1c_svv_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|Delete|容器|保存Delete Multiple Object请求的容器。子节点：一个或多个Object元素，可选的Quiet元素

父节点： None

|
|Key|字符串|被删除Object的名字。父节点：Object

|
|Object|容器|保存一个Object信息的容器。子节点：key

父节点：Delete

|
|Quiet|枚举字符串|打开“简单”响应模式的开关。有效值：true、false

默认值：false

父节点：Delete

|

## 响应元素\(Response Elements\) {#section_ync_yyv_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|Deleted|容器|保存被成功删除的Object的容器。子节点：Key

父节点：DeleteResult

|
|DeleteResult|容器|保存Delete Multiple Object请求结果的容器。子节点：Deleted

父节点：None

|
|Key|字符串|OSS执行删除操作的Object名字。父节点：Deleted

 |
|EncodingType|字符串|指明返回结果中编码使用的类型。如果请求的参数中指定了encoding-type，那返回的结果会对Key进行编码。父节点：容器

|

## 细节分析 {#section_xfy_rzv_wdb .section}

-   Delete Multiple Objects请求必须填Content-Length和Content-MD5字段。OSS会根据这些字段验证收到的消息体是正确的，之后才会执行删除操作。
-   生成Content-MD5字段内容方法：首先将Delete Multiple Object请求内容经过MD5加密后得到一个128位字节数组；再将该字节数组用base64算法编码；最后得到的字符串即是Content-MD5字段内容。
-   Delete Multiple Objects请求默认是详细\(verbose\)模式。
-   在Delete Multiple Objects请求中删除一个不存在的Object，仍然认为是成功的。
-   Delete Multiple Objects的消息体最大允许2MB的内容，超过2MB会返回MalformedXML错误码。
-   Delete Multiple Objects请求最多允许一次删除1000个Object；超过1000个Object会返回MalformedXML错误码。
-   如果用户上传了Content-MD5请求头，OSS会计算body的Content-MD5并检查一致性，如果不一致，将返回InvalidDigest错误码。

## 示例 {#section_mcs_21w_wdb .section}

**请求示例I：**

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

**返回示例：**

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

**请求示例II：**

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

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Feb 2012 12:33:45 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

