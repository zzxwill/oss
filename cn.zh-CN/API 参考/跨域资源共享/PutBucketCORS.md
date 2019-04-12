# PutBucketCORS {#reference_wtg_ttc_xdb .reference}

PutBucketcors接口用于为指定的存储空间（Bucket）设定一个跨域资源共享（CORS）规则。如果Bucket已有CORS规则，使用此接口会覆盖原有规则。Bucket默认不开启CORS功能，所有跨域请求的Origin都不被允许。

**说明：** 

-   如需在应用程序中使用CORS功能，需要通过本接口手动上传CORS规则来开启CORS功能。例如，从www.a.com通过浏览器的XMLHttpRequest功能来访问OSS，需要通过本接口手动上传CORS规则。CORS规则需由XML文档进行描述。
-   当OSS收到一个跨域请求或OPTIONS请求，会先读取Bucket对应的CORS规则，然后进行相应的权限检查。OSS会依次检查每一条规则，使用第一条匹配的规则来允许请求并返回对应的header。如果所有规则都匹配失败则不附加任何CORS相关的header。
-   每个Bucket的CORS设定是由多条CORS规则指定的。每个Bucket最多允许10条规则。上传的XML文档最多允许16 KB大小。
-   CORS规则匹配成功必须满足以下三个条件：
    -   请求的Origin必须匹配一个AllowedOrigin项。
    -   请求的方法（如GET、PUT等）或者OPTIONS请求的Access-Control-Request-Method头对应的方法必须匹配一个AllowedMethod项。
    -   OPTIONS请求的Access-Control-Request-Headers头包含的每个header都必须匹配一个AllowedHeader项。

## 请求语法 {#section_qw4_xtc_xdb .section}

```
PUT /?cors HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue

<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration>
    <CORSRule>
      <AllowedOrigin>the origin you want allow CORS request from</AllowedOrigin>
      <AllowedOrigin>…</AllowedOrigin>
      <AllowedMethod>HTTP method</AllowedMethod>
      <AllowedMethod>…</AllowedMethod>
        <AllowedHeader> headers that allowed browser to send</AllowedHeader>
          <AllowedHeader>…</AllowedHeader>
          <ExposeHeader> headers in response that can access from client app</ExposeHeader>
          <ExposeHeader>…</ExposeHeader>
          <MaxAgeSeconds>time to cache pre-fight response</MaxAgeSeconds>
    </CORSRule>
    <CORSRule>
      …
    </CORSRule>
…
</CORSConfiguration >
```

## 请求元素 {#section_gvt_c5c_xdb .section}

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|CORSRule|容器|是| CORS规则的容器。

 **说明：** 每个Bucket最多允许10条CORS规则。

 父节点：CORSConfiguration

 |
|AllowedOrigin|字符串|是| 指定允许的跨域请求的来源。

 **说明：** 

-   OSS允许使用多个元素来指定多个允许的来源。
-   OSS允许使用最多一个星号（\*）通配符。如果指定为星号（\*），则表示允许所有的来源的跨域请求。

 父节点：CORSRule

 |
|AllowedMethod|枚举（GET,PUT,DELETE,POST,HEAD）|是| 指定允许的跨域请求方法。

 父节点：CORSRule

 |
|AllowedHeader|字符串|否| 控制在OPTIONS预取指令中Access-Control-Request-Headers头中指定的header是否允许。在Access-Control-Request-Headers中指定的每个header都必须在AllowedHeader中有一条对应的项。

 **说明：** 允许最多使用一个星号（\*）通配符 。

 父节点：CORSRule

 |
|ExposeHeader|字符串|否| 指定允许用户从应用程序中访问的响应头。例如，一个Javascript的XMLHttpRequest对象。

 **说明：** 不允许使用星号（\*）通配符。

 父节点：CORSRule

 |
|MaxAgeSeconds|整型|否| 指定浏览器对特定资源的预取（OPTIONS）请求返回结果的缓存时间。单位为秒。

 **说明：** 一条CORS规则最多允许出现一个MaxAgeSeconds。

 父节点：CORSRule

 |
|CORSConfiguration|容器|是| Bucket的CORS规则容器。

 父节点：无

 |

## 示例 {#section_mlh_rvc_xdb .section}

**请求示例**

```
PUT /?cors HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length: 186
Date: Fri, 04 May 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=

<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration>
    <CORSRule>
      <AllowedOrigin>*</AllowedOrigin>
      <AllowedMethod>PUT</AllowedMethod>
      <AllowedMethod>GET</AllowedMethod>
      <AllowedHeader>Authorization</AllowedHeader>
    </CORSRule>
    <CORSRule>
      <AllowedOrigin>http://www.a.com</AllowedOrigin>
      <AllowedOrigin>http://www.b.com</AllowedOrigin>
      <AllowedMethod>GET</AllowedMethod>
      <AllowedHeader> Authorization</AllowedHeader>
      <ExposeHeader>x-oss-test</ExposeHeader>
      <ExposeHeader>x-oss-test1</ExposeHeader>
      <MaxAgeSeconds>100</MaxAgeSeconds>
    </CORSRule>
</CORSConfiguration >
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 50519080C4689A033D00235F
Date: Fri, 04 May 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/跨域资源共享.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/跨域资源共享.md)
-   [PHP](../../../../../cn.zh-CN/SDK 参考/PHP/跨域资源共享.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/跨域资源共享.md)
-   [C++](../../../../../cn.zh-CN/SDK 参考/C++/跨域资源共享.md)
-   [C](../../../../../cn.zh-CN/SDK 参考/C/跨域资源共享.md)
-   [.NET](../../../../../cn.zh-CN/SDK 参考/.NET/跨域资源共享.md)
-   [Ruby](../../../../../cn.zh-CN/SDK 参考/Ruby/跨域资源共享.md)

## 错误码 {#section_nfp_nfc_5gb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|InvalidDigest|400|上传了Content-MD5请求头后，OSS会计算消息体的Content-MD5并检查一致性，如果不一致会返回此错误码。|

