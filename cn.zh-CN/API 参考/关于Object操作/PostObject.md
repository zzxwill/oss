# PostObject {#reference_smp_nsw_wdb .reference}

PostObject使用HTML表单上传文件到指定bucket。

Post作为Put的替代品，使得基于浏览器上传文件到bucket成为可能。Post Object的消息实体通过多重表单格式（multipart/form-data）编码，在Put Object操作中参数通过HTTP请求头传递，在Post操作中参数则作为消息实体中的表单域传递。

## Post object {#section_xwr_rsw_wdb .section}

**请求语法**

```
POST / HTTP/1.1 
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
User-Agent: browser_data
Content-Length：ContentLength
Content-Type: multipart/form-data; boundary=9431149156168
--9431149156168
Content-Disposition: form-data; name="key"
key
--9431149156168
Content-Disposition: form-data; name="success_action_redirect"
success_redirect
--9431149156168
Content-Disposition: form-data; name="Content-Disposition"
attachment;filename=oss_download.jpg
--9431149156168
Content-Disposition: form-data; name="x-oss-meta-uuid"
myuuid
--9431149156168
Content-Disposition: form-data; name="x-oss-meta-tag"
mytag
--9431149156168
Content-Disposition: form-data; name="OSSAccessKeyId"
access-key-id
--9431149156168
Content-Disposition: form-data; name="policy"
encoded_policy
--9431149156168
Content-Disposition: form-data; name="Signature"
signature
--9431149156168
Content-Disposition: form-data; name="file"; filename="MyFilename.jpg"
Content-Type: image/jpeg
file_content
--9431149156168
Content-Disposition: form-data; name="submit"
Upload to OSS
--9431149156168--
```

**表单域**

|名称|类型|描述|必须|
|:-|:-|:-|:-|
|OSSAccessKeyId|字符串|Bucket 拥有者的Access Key Id。默认值：无

限制：当bucket非public-read-write或者提供了policy（或Signature）表单域时，必须提供该表单域。

|有条件的|
|policy|字符串|policy规定了请求的表单域的合法性。不包含policy表单域的请求被认为是匿名请求，并且只能访问public-read-write的bucket。更详细描述请参考下文 Post Policy。默认值：无

限制：当bucket非public-read-write或者提供了OSSAccessKeyId（或Signature）表单域时，必须提供该表单域。

|有条件的|
|Signature|字符串|根据Access Key Secret和policy计算的签名信息，OSS验证该签名信息从而验证该Post请求的合法性。更详细描述请参考下文 Post Signature。默认值：无

限制：当bucket非public-read-write或者提供了OSSAccessKeyId（或policy）表单域时，必须提供该表单域。

|有条件的|
|Cache-Control, Content-Type, Content-Disposition, Content-Encoding, Expires|字符串|REST请求头，更多的信息见Put Object。 默认值：无

|可选|
|file|字符串|文件或文本内容，必须是表单中的最后一个域。浏览器会自动根据文件类型来设置Content-Type，会覆盖用户的设置。 OSS一次只能上传一个文件。默认值：无

|必须|
|key|字符串|上传文件的object名称。 如果需要使用用户上传的文件名称作为object名，使用$\{filename\}变量。例如：如果用户上传了文件b.jpg，而key域的值设置为/user/a/$\{filename\}，最终的object名为/user/a/b.jpg。 如果文件名包含路径，则去除文件名中的路径，例如用户上传了文件a/b/c/b.jpg，则取文件名为b.jpg，若key域的值设置为/user/a/$\{filename\}，最终的object名为/user/a/b.jpg 默认值：无

 |必须|
|success\_action\_redirect|字符串|上传成功后客户端跳转到的URL，如果未指定该表单域，返回结果由success\_action\_status表单域指定。如果上传失败，OSS返回错误码，并不进行跳转。默认值：无

|可选|
|success\_action\_status|字符串|未指定success\_action\_redirect表单域时，该表单域指定了上传成功后返回给客户端的状态码。 接受值为200, 201, 204（默认）。 如果该域的值为200或者204，OSS返回一个空文档和相应的状态码。 如果该域的值设置为201，OSS返回一个XML文件和201状态码。 如果其值未设置或者设置成一个非法值，OSS返回一个空文档和204状态码。默认值：无

| |
|x-oss-meta-\*|字符串|用户指定的user meta值。 OSS不会检查或者使用该值。默认值：无

|可选|
|x-oss-server-side-encryption|字符串|指定OSS创建object时的服务器端加密编码算法。合法值：AES256

|可选|
|x-oss-object-acl|字符串|指定oss创建object时的访问权限。合法值：public-read，private，public-read-write

|可选|
|x-oss-security-token|字符串|若本次访问是使用STS临时授权方式，则需要指定该项为SecurityToken的值，同时OSSAccessKeyId需要使用与之配对的临时AccessKeyId，计算签名时，与使用普通AccessKeyId签名方式一致。默认值：无

|可选|

**响应Header**

|名称|类型|描述|
|:-|:-|:-|
|x-oss-server-side-encryption|字符串|如果请求指定了x-oss-server-side-encryption熵编码，则响应Header中包含了该头部，指明了所使用的加密算法。|

响应元素\(Response Elements\)

|名称|类型|描述|
|:-|:-|:-|
|PostResponse|容器|保持Post请求结果的容器。子节点：Bucket, ETag, Key, Location

|
|Bucket|字符串|Bucket名称。父节点：PostResponse

|
|ETag|字符串|ETag \(entity tag\) 在每个Object生成的时候被创建，Post请求创建的Object，ETag值是该Object内容的uuid，可以用于检查该Object内容是否发生变化。父节点：PostResponse

|
|Location|字符串|新创建Object的URL。父节点：PostResponse

|

**细节分析**

-   进行Post操作要求对bucket有写权限，如果bucket为public-read-write，可以不上传签名信息，否则要求对该操作进行签名验证。与Put操作不同，Post操作使用AccessKeySecret对policy进行签名计算出签名字符串作为Signature表单域的值，OSS会验证该值从而判断签名的合法性。
-   无论bucket是否为public-read-write，一旦上传OSSAccessKeyId, policy, Signature表单域中的任意一个，则另两个表单域为必选项，缺失时OSS会返回错误码：InvalidArgument。
-   post操作提交表单编码必须为“multipart/form-data”，即header中Content-Type为`multipart/form-data`;`boundary=xxxxxx` 这样的形式，boundary为边界字符串。
-   提交表单的URL为bucket域名即可，不需要在URL中指定object。即请求行是`POST / HTTP/1.1`，不能写成`POST /ObjectName HTTP/1.1`。
-   policy规定了该次Post请求中表单域的合法值，OSS会根据policy判断请求的合法性，如果不合法会返回错误码：AccessDenied。在检查policy合法性时，policy中不涉及的表单域不进行检查。
-   表单和policy必须使用UTF-8编码，policy为经过UTF-8编码和base64编码的JSON。
-   Post请求中可以包含额外的表单域，OSS会根据policy对这些表单域检查合法性。
-   如果用户上传了Content-MD5请求头，OSS会计算body的Content-MD5并检查一致性，如果不一致，将返回InvalidDigest错误码。
-   如果POST请求中包含Header签名信息或URL签名信息，OSS不会对它们做检查。
-   如果请求中携带以x-oss-meta-为前缀的表单域，则视为user meta，比如x-oss-meta-location。一个Object可以有多个类似的参数，但所有的user meta总大小不能超过8k。
-   Post请求的body总长度不允许超过5G。若文件长度过大，会返回错误码：EntityTooLarge。
-   如果上传指定了x-oss-server-side-encryption Header请求域，则必须设置其值为AES256，否则会返回400和错误码：InvalidEncryptionAlgorithmError。指定该Header后，在响应头中也会返回该Header，OSS会对上传的Object进行加密编码存储，当这个Object被下载时，响应头中会包含x-oss-server-side-encryption，值被设置成该Object的加密算法。
-   表单域为大小写不敏感的，但是表单域的值为大小写敏感的。

**示例**

-   请求示例：

    ```
    POST / HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 344606
    Content-Type: multipart/form-data; boundary=9431149156168
    --9431149156168
    Content-Disposition: form-data; name="key"
    /user/a/${filename}
    --9431149156168
    Content-Disposition: form-data; name="success_action_status"
    200
    --9431149156168
    Content-Disposition: form-data; name="Content-Disposition"
    content_disposition
    --9431149156168
    Content-Disposition: form-data; name="x-oss-meta-uuid"
    uuid
    --9431149156168
    Content-Disposition: form-data; name="x-oss-meta-tag"
    metadata
    --9431149156168
    Content-Disposition: form-data; name="OSSAccessKeyId"
    44CF9590006BF252F707
    --9431149156168
    Content-Disposition: form-data; name="policy"
    eyJleHBpcmF0aW9uIjoiMjAxMy0xMi0wMVQxMjowMDowMFoiLCJjb25kaXRpb25zIjpbWyJjb250ZW50LWxlbmd0aC1yYW5nZSIsIDAsIDEwNDg1NzYwXSx7ImJ1Y2tldCI6ImFoYWhhIn0sIHsiQSI6ICJhIn0seyJrZXkiOiAiQUJDIn1dfQ==
    --9431149156168
    Content-Disposition: form-data; name="Signature"
    kZoYNv66bsmc10+dcGKw5x2PRrk=
    --9431149156168
    Content-Disposition: form-data; name="file"; filename="MyFilename.txt"
    Content-Type: text/plain
    abcdefg
    --9431149156168
    Content-Disposition: form-data; name="submit"
    Upload to OSS
    --9431149156168--
    ```

-   返回示例：

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 61d2042d-1b68-6708-5906-33d81921362e 
    Date: Fri, 24 Feb 2014 06:03:28 GMT
    ETag: 5B3C1A2E053D763E1B002CC607C5A0FE
    Connection: keep-alive
    Content-Length: 0  
    Server: AliyunOSS
    ```


## Post Policy {#section_d5z_1ww_wdb .section}

Post请求的policy表单域用于验证请求的合法性。 policy为一段经过UTF-8和base64编码的JSON文本，声明了Post请求必须满足的条件。虽然对于public-read-write的bucket上传时，post表单域为可选项，我们强烈建议使用该域来限制Post请求。

**policy示例**

```
{ "expiration": "2014-12-01T12:00:00.000Z",
  "conditions": [
    {"bucket": "johnsmith" },
    ["starts-with", "$key", "user/eric/"]
  ]
}
```

Post policy中必须包含expiration和condtions。

**Expiration**

Expiration项指定了policy的过期时间，以ISO8601 GMT时间表示。例如”2014-12-01T12:00:00.000Z”指定了Post请求必须发生在2014年12月1日12点之前。

**Conditions**

Conditions是一个列表，可以用于指定Post请求的表单域的合法值。注意：表单域对应的值在检查policy之后进行扩展，因此，policy中设置的表单域的合法值应当对应于扩展之前的表单域的值。Policy中支持的conditions项见下表：

|名称|描述|
|:-|:-|
|content-length-range|上传文件的最小和最大允许大小。 该condition支持contion-length-range匹配方式。|
|Cache-Control, Content-Type, Content-Disposition, Content-Encoding, Expires|HTTP请求头。 该condition支持精确匹配和starts-with匹配方式。|
|key|上传文件的object名称。 该condition支持精确匹配和starts-with匹配方式。|
|success\_action\_redirect|上传成功后的跳转URL地址。 该condition支持精确匹配和starts-with匹配方式。|
|success\_action\_status|未指定success\_action\_redirect时，上传成功后的返回状态码。 该condition支持精确匹配和starts-with匹配方式。|
|x-oss-meta-\*|用户指定的user meta。 该condition支持精确匹配和starts-with匹配方式。|

如果Post请求中包含其他的表单域，可以将这些额外的表单域加入到policy的conditions中,conditions不涉及的表单域将不会进行合法性检查。

**Conditions匹配方式**

|Conditions匹配方式|描述|
|:-------------|:-|
|精确匹配|表单域的值必须精确匹配conditions中声明的值。如指定key表单域的值必须为a： \{“key”: “a”\} 同样可以写为： \[“eq”, “$key”, “a”\]|
|Starts With|表单域的值必须以指定值开始。例如指定key的值必须以/user/user1开始： \[“starts-with”, “$key”, “/user/user1”\]|
|指定文件大小|指定所允许上传的文件最大大小和最小大小，例如允许的文件大小为1到10字节： \[“content-length-range”, 1, 10\]|

**转义字符**

于在 Post policy 中 $ 表示变量，所以如果要描述 $，需要使用转义字符\\$。除此之外，JSON 将对一些字符进行转义。下图描述了 Post policy 的 JSON 中需要进行转义的字符。

|转义字符|描述|
|:---|:-|
|\\/|斜杠|
|\\|反斜杠|
|\\”|双引号|
|\\$|美元符|
|\\b|空格|
|\\f|换页|
|\\n|换行|
|\\r|回车|
|\\t|水平制表符|
|\\uxxxx|Unicode 字符|

## Post Signature {#section_wny_mww_wdb .section}

对于验证的Post请求，HTML表单中必须包含policy和Signature信息。policy控制请求中那些值是允许的。计算Signature的具体流程为：

1.  创建一个 UTF-8 编码的 policy。
2.  将 policy 进行 base64 编码，其值即为 policy 表单域该填入的值，将该值作为将要签名的字符串。
3.  使用 AccessKeySecret 对要签名的字符串进行签名，签名方法与Head中签名的计算方法相同（将要签名的字符串替换为 policy 即可），请参见在Header中包含签名。

## 示例 Demo {#section_ipm_rww_wdb .section}

Web 端表单直传 OSS 示例 Demo，请参见[JavaScript客户端签名直传](https://help.aliyun.com/document_detail/31925.html)。

