# Post Object错误及排查 {#concept_nrx_jfj_wdb .concept}

## Post Object简介 {#section_mcj_kfj_wdb .section}

Post Object使用表单上传文件到OSS。Post Object的消息实体通过多重表单格式multipart/form-data编码，详细说明请参看[RFC 2388](https://tools.ietf.org/html/rfc2388)。PutObject中参数通过HTTP请求头传递，Post Object参数则作为消息体的表单域传递。

Post Object消息包括消息头（Header）和消息体（Body）。Header和Body之间，由`\r\n--{boundary}`分割。Body由一系列的表单域构成，表单域格式如下：

`Content-Disposition: form-data; name="{key}"\r\n\r\n{value}\r\n--{boundary}`

常见的Header有Host、User-Agent、Content-Length、Content-Type、Content-MD5等，表单域字段有key、OSSAccessKeyId、Signature、Content-Disposition、object meta\(x-oss-meta-\*\)、x-oss-security-token、其它HTTP Header\(Cache-Control/Content-Type/Cache-Control/Content-Type/Content-Disposition/Content-Encoding/Expires/Content-Encoding/Expires\)、file等。表单域中`file`必须是最后一个。

更多详细的介绍请参看[Post Object](../../../../intl.zh-CN/API 参考/关于Object操作/PostObject.md#)。

## Post Object常见错误 {#section_uxq_lfj_wdb .section}

Post Object常见错误见下表：

|序号|错误|原因|解法|
|:-|:-|:-|:-|
|1|ErrorCode: MalformedPOSTRequest ErrorMessage: The body of your POST request is not well-formed multipart/form-data|表单域格式不符合要求|表单域格式请参看表后的 Post Object表单域格式|
|2|ErrorCode: InvalidAccessKeyId ErrorMessage: The OSS Access Key Id you provided does not exist in our records.|`AccessKeyID`禁用或不存在，或者过期的临时用户AccessKeyID，或者临时用户没有提供STS Token|排查方法请参看[InvalidAccessKeyId错误排查](intl.zh-CN/常见错误排除/OSS 403错误及排查.md#)|
|3|ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy expired.|表单域policy中的`expiration`过期|请调整policy中的`expiration`，注意expiration的格式 ISO8601 GMT|
|4|ErrorCode: AccessDenied ErrorMessage: SignatureDoesNotMatch The request signature we calculated does not match the signature you provided. Check your key and signing method.|签名错误|签名方法请参看下面的Post Object的签名|
|5|ErrorCode: InvalidPolicyDocument ErrorMessage: Invalid Policy: Invalid Simple-Condition: Simple-Conditions must have exactly one property specified.|请求中policy至少包含一个condition|请参看表后的Post Object的Policy格式|
|6|ErrorCode: InvalidPolicyDocument ErrorMessage: Invalid Policy: Invalid JSON: unknown char e|请求中的`policy`格式不正确|请检查policy格式，`"`是否加缺失，转义字符是否加`\`|
|7|ErrorCode: InvalidPolicyDocument ErrorMessage: Invalid Policy: Invalid JSON: , or \] expected|请求中的`policy`格式不正确|请检查policy中是否缺少`,`或`]`|
|8|ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy Condition failed: \[“starts-with”, “$key”, “user/eric/“\]|请求指定的`key`与`policy`限定的不符|请检查请求中表单域`key`的值|
|9|ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy Condition failed: \[“eq”, “$bucket”, “mingdi-bjx”\]|请求指定`bucket`与`policy`限定的不符|请检查endpoint中的`bucket`的值|
|10|ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy Condition failed: \[“starts-with”, “$x-oss-meta-prop”, “prop-“\]|请求指定的文件元数据`x-oss-meta-prop`与policy限定的不符|请检查请求中的`x-oss-meta-prop`的值|
|11|ErrorCode: AccessDenied ErrorMessage: Invalid according to Policy: Policy Condition failed: \[“eq”, “$\{field\}”, “$\{value\}”\]|表单域中指定的`{field}`与policy中限定的值不符，或者在请求中没有指定|请检查请求中的`{field}`的值|
|12|ErrorCode: AccessDenied ErrorMessage: You have no right to access this object because of bucket acl.|当前用户无权限|请参看[OSS权限问题及排查](intl.zh-CN/常见错误排除/OSS 权限问题及排查.md#)|
|13|ErrorCode: InvalidArgument ErrorMessage: The bucket POST must contain the specified ‘key’. If it is specified, please check the order of the fields|表单域没有指定`key`，或者放在了表单域`file`后|请添加表单域`key`或调整顺序|

-   Post Object表单域格式

    Post Object请求格式，有以下注意点：

    -   Header一定要有`Content-Type: multipart/form-data; boundary={boundary}`。
    -   Header和body之间由`\r\n--{boundary}` 分割。
    -   表单域格式 ：

        ```
        Content-Disposition: form-data;
                name="{key}"\r\n\r\n{value}\r\n--{boundary}
        ```

    -   表单域名称大小写敏感，如policy、key、file、OSSAccessKeyId、OSSAccessKeyId、Content-Disposition。

        **说明：** 表单域`file`必须为最后一个表单域。

    -   Bucket为`public-read-write`时，可以不指定表单域OSSAccessKeyId、policy、Signature，一旦指定OSSAccessKeyId、policy、 Signature中的任意一个，无论bucket是否为public-read-write，则另两个必须指定。
    下面是Post Object请求的示例：

    ```
    POST / HTTP/1.1
    User-Agent: Mozilla/5.0 (Windows; U; Windows NT 6.1; zh-CN; rv:1.9.2.6)
    Content-Type: multipart/form-data; boundary=9431149156168
    Host: mingdi-hz.oss-cn-hangzhou.aliyuncs.com
    Accept: text/html, image/gif, image/jpeg, *; q=.2, */*; q=.2
    Connection: keep-alive
    Content-Length: 5052
    --9431149156168
    Content-Disposition: form-data; name="key"
    test-key
    --9431149156168
    Content-Disposition: form-data; name="Content-Disposition"
    attachment;filename=D:\img\1.png
    --9431149156168
    Content-Disposition: form-data; name="OSSAccessKeyId"
    2NeL********j2Eb
    ```

    **说明：** 

    -   上面示例请求中`\r\n`显示为新行，即换行，后面的示例请求类似。
    -   上面的示例为请求的部分内容，完整的请求请参看[Post Object](../../../../intl.zh-CN/API 参考/关于Object操作/PostObject.md#)。
    如果您还有疑问，请参考示例代码：

    -   [C\#](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PostPolicySample.cs)
    -   [Java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/PostObjectSample.java)
    -   [JS](../../../../intl.zh-CN/最佳实践/Web端上传数据至OSS/Web端PostObject直传实践/JavaScript客户端签名直传.md#)
-   Post Object的Policy格式

    Post Object请求的`policy`表单域用于验证请求的合法性，声明了Post Object请求必须满足的条件。限定条件为：

    -   UTF-8格式的Json文本，经过base64编码后放在表单域policy中。
    -   Policy中必须包含expiration和condtions，其中condtions至少有一项。
    下面是base64编码前的policy示例：

    ```
    {
        "expiration": "2018-01-01T12:00:00.000Z",
        "conditions": [
            ["content-length-range", 0, 104857600]
        ]
    }
    ```

    `expiration`项指定了请求的过期时间， ISO8601 GMT 时间格式；如 `2018-01-01T12:00:00.000Z`指定请求必须发生在2018年1月1日12点前。

    Post Policy支持的限定条件（Conditions）如下：

    |名称|描述|示例|
    |:-|:-|:-|
    |bucket|上传文件的Bucket名称。支持精确匹配方式。|\{“bucket”: “johnsmith” \} 或 \[“eq”, “$bucket”, “johnsmith”\]|
    |key|上传文件的名称。支持精确匹配和前缀匹配方式。|\[“starts-with”, “$key”, “user/etc/\]”|
    |content-length-range|上传文件允许的最小、最大长度。|\[“content-length-range”, 0, 104857600\]|
    |x-oss-meta-\*|指定的object meta。支持精确匹配和前缀匹配方式。|\[“starts-with”, “$x-oss-meta-prop”, “prop-“\]|
    |success\_action\_redirect|上传成功后的跳转URL地址。支持精确匹配和前缀匹配方式。|\[“starts-with”, “$success\_action\_redirect”, “[http://www.aliyun.com](http://www.aliyun.com/)”\]|
    |success\_action\_status|未指定success\_action\_redirect时，上传成功后的返回状态码。支持精确匹配和前缀匹配方式。|\[“eq”, “$success\_action\_status”, “204”\]|
    |Cache-Control, Content-Type, Content-Disposition, Content-Encoding, Expires等|HTTP请求头，作为表单域传递。支持精确匹配和前缀匹配方式。|\[“eq”, “$Content-Encoding”, “ZLIB”\]|

    Post Policy有以下转义字符，使用 `\` 转义。

    |转义字符|描述|
    |:---|:-|
    |/|斜杠|
    |\\|反斜杠|
    |“|双引号|
    |$|美元符|
    |\\b|空格|
    |\\f|换页|
    |\\n|换行|
    |\\r|回车|
    |\\t|水平制表符|
    |\\uxxxx|Unicode字符|

    Post Policy更详细的说明，请参考[Post Policy](../../../../intl.zh-CN/API 参考/关于Object操作/PostObject.md#section_d5z_1ww_wdb)。

-   Post Object的签名

    对于验证的Post请求，请求中必须包含AccessKeyID、policy、Signature表单域。计算签名的流程如下：

    1.  创建一个`UTF-8`编码的policy。
    2.  将policy进行`base64`编码，其值即为policy表单域该填入的值，并将该值作为将要签名的字符串。
    3.  使用`AccessKeySecret`对要签名的字符串进行签名，先用hmac-sha1哈希，再base64编码；签名方法与[Header签名](../../../../intl.zh-CN/API 参考/访问控制/在Header中包含签名.md#)的方法相同。
    即：

    ```
    Signature = base64(hmac-sha1(AccessKeySecret, base64(policy)))
    ```

    计算出的签名在表单域`Signature`中指定，如下所示：

    ```
    Content-Disposition: form-data; name="Signature"
    {signature}
    --9431149156168
    ```

    如果您还有疑问，请参考示例代码：

    -   [C\#](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PostPolicySample.cs)
    -   [Java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/PostObjectSample.java)
    -   [JS](../../../../intl.zh-CN/最佳实践/Web端上传数据至OSS/Web端PostObject直传实践/JavaScript客户端签名直传.md#)

## 常见问题 {#section_w3h_jgj_wdb .section}

-   怎么指定key？

    key即object name，在表单域`key`中指定，示例如下：

    ```
    Content-Disposition: form-data; name="key"
    {key}
    --9431149156168
    ```

-   怎么指定object内容？

    Object内容通过表单域`file`中指定，示例如下：

    ```
    Content-Disposition: form-data; name="file"; filename="images.png"
    Content-Type: image/png
    {file-content}
    --9431149156168
    ```

    **说明：** 

    -   表单域`file`必须是表单中的最后一个域，即表单域`file`必须放在所有表单域后。
    -   `filename`是上传的本地文件名称，而不是object name。
-   怎么指定object类型content-type？

    Object类型在表单域`file`中指定`Content-Type`，而不是Header中的`Content-Type`，示例如下：

    ```
    Content-Disposition: form-data; name="file"; filename="images.png"
    Content-Type: image/png
    {file-content}
    --9431149156168
    ```

-   怎么指定object内容md5校验content-md5？

    在Post Object请求头中指定`Content-MD5`，注意MD5值是整个body的MD5，即所有表单域的MD5。请求Header示例如下：

    ```
    POST / HTTP/1.1
    User-Agent: Mozilla/5.0 (Windows; U; Windows NT 6.1; zh-CN; rv:1.9.2.6)
    Content-Type: multipart/form-data; boundary=9431149156168
    Content-MD5: tdqHe4hT/TuKb7Y4by+nJg==
    Host: mingdi-hz.oss-cn-hangzhou.aliyuncs.com
    Accept: text/html, image/gif, image/jpeg, *; q=.2, */*; q=.2
    Connection: keep-alive
    Content-Length: 5246
    --9431149156168
    ```

-   怎么指定签名Signature？

    签名的计算方法请参看`PostObject中的签名` ， 签名通过表单域`Signature`携带。

-   怎么使用临时用户STS Token执行Post Object？

    临时用户密钥的AccessKeyID、AccessKeySecret用法跟主用户、子用户相同，Token放在表单域`x-oss-security-token`中携带。示例如下：

    ```
    Content-Disposition: form-data; name="Signature"
    5L0+KaeugxYygfqWLJLoy0ehOmA=
    --9431149156168
    Content-Disposition: form-data; name="x-oss-security-token"
    {Token}
    --9431149156168
    ```

    **说明：** 如果您想了解更多访问控制的信息，请参看[阿里云访问控制](https://yq.aliyun.com/articles/58413?spm=a2c4g.11186623.2.17.ogoRhr)。

-   怎么指定上传回调（callback）？

    上传回调\(callback\)通过表单域`callback`携带。示例如下：

    ```
    Content-Disposition: form-data; name="callback"
    eyJjYWxsYmFja0JvZHlUeXBlIjogImFwcGxpY2F0aW9uL3gtd3d3LWZvcm0tdXJsZW5jb2RlZCIsICJjYWxsYmFja0JvZHkiOiAiZmlsZW5hbWU9JHtvYmplY3R9JnNpemU9JHtzaXplfSZtaW1lVHlwZT0ke21pbWVUeXBlfSIsICJjYWxsYmFja1VybCI6ICJodHRwOi8vb3NzLWRlbW8uYWxpeXVuY3MuY29tOjIzNDUwIn0=
    --9431149156168
    ```

    Callback的自定义参数也是通过表单域携带。示例如下：

    ```
    Content-Disposition: form-data; name="x:var1"
    {var1-value}
    --9431149156168
    ```

    **说明：** 如果您想了解callback更多内容，请参看[上传回调](../../../../intl.zh-CN/API 参考/关于Object操作/Callback.md#)。

-   怎么指定Content-Transfer-Encoding？

    在表单域`file`中指定`Content-Transfer-Encoding`。`file`表单域示例如下：

    ```
    Content-Disposition: form-data; name="file"; filename="images.png"
    Content-Type: image/png
    Content-Transfer-Encoding: base64
    {file-content}
    --9431149156168
    ```

-   怎么指定用户自定义元信息Object User Meta？

    用户自定义元信息，可以通过表单域指定，示例如下：

    ```
    Content-Disposition: form-data; name="x-oss-meta-uuid"
    {uuid}
    --9431149156168
    Content-Disposition: form-data; name="x-oss-meta-tag"
    {tag}
    --9431149156168
    ```

    **说明：** 文件元信息更详细的说明，请参看[文件元信息Object Meta](../../../../intl.zh-CN/开发指南/管理文件/管理文件元信息.md#)。

-   怎么指定限定条件expiration、Key、Bucket、size、header等?

    OSS的Post Object支持丰富的条件限制，可以满足高安全性要求。限定条件Conditions可以通过表单域`policy` 指定，详细的说明请参看上面的Post Object的Policy格式。下面是一个policy的示例：

    ```
    {
        "expiration": "2018-01-01T12:00:00.000Z",
        "conditions": [
            ["eq", "$bucket", "md-hz"],
            ["starts-with", "$key", "md/conf/"],
            ["content-length-range", 0, 104857600]
        ]
    }
    ```

    上面的policy实例中，用户的Post Object操作的限定条件如下：

    -   `bucket`必须是`md-hz`。
    -   `key`必须以`md/conf/`开头。
    -   上传的文件长度必须在100M以下。
    -   请求时间在`2018-01-01T12:00:00.000Z`之前。
-   怎么指定Cache-Control、Content-Type、Content-Disposition、Content-Encoding、Expires等HTTP Header？

    `Cache-Control``Content-Type`、`Content-Disposition、``Content-Encoding``Expires`等HTTP Header需要在表单域中指定，这些HTTP Header的含义请参看[RFC2616](https://tools.ietf.org/html/rfc2616?spm=a2c4g.11186623.2.20.ogoRhr) 。但是`Content-MD5`需要在Post Header中指定。


## Post Object示例 {#section_o31_5hj_wdb .section}

-   [C\# Post Demo](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PostPolicySample.cs?spm=a2c4g.11186623.2.21.ogoRhr&file=PostPolicySample.cs)
-   [Java Post Demo](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/PostObjectSample.java?spm=a2c4g.11186623.2.22.ogoRhr&file=PostObjectSample.java)
-   [JavaScript Post Demo](../../../../intl.zh-CN/最佳实践/Web端上传数据至OSS/Web端PostObject直传实践/JavaScript客户端签名直传.md#)

## 常用链接 {#section_tfy_5hj_wdb .section}

-   [Post Object](../../../../intl.zh-CN/API 参考/关于Object操作/PostObject.md#)
-   [Java PostObject实现](https://yq.aliyun.com/articles/30346?spm=a2c4g.11186623.2.25.ogoRhr)

