# PutBucketcors {#reference_wtg_ttc_xdb .reference}

PutBucketcors用于在指定的bucket上设定一个跨域资源共享\(CORS\)的规则，如果原规则存在则覆盖原规则。

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

## 请求元素（Request Elements） {#section_gvt_c5c_xdb .section}

|名称|类型|描述|是否必须|
|:-|:-|:-|:---|
|CORSRule|容器|CORS规则的容器，每个bucket最多允许10条规则父节点：CORSConfiguration

|是|
|AllowedOrigin|字符串|指定允许的跨域请求的来源，允许使用多个元素来指定多个允许的来源。 允许使用最多一个“\*”通配符。如果指定为“\*”则表示允许所有的来源的跨域请求。 父节点：CORSRule

 |是|
|AllowedMethod|枚举（GET,PUT,DELETE,POST,HEAD）|指定允许的跨域请求方法。父节点：CORSRule

|是|
|AllowedHeader|字符串|控制在OPTIONS预取指令中Access-Control-Request-Headers头中指定的header是否允许。在Access-Control-Request-Headers中指定的每个header都必须在AllowedHeader中有一条对应的项。允许使用最多一个“\*”通配符 。父节点：CORSRule

|否|
|ExposeHeader|字符串|指定允许用户从应用程序中访问的响应头（例如一个Javascript的XMLHttpRequest对象。）不允许使用“\*”通配符。父节点：CORSRule

|否|
|MaxAgeSeconds|整型|指定浏览器对特定资源的预取（OPTIONS）请求返回结果的缓存时间，单位为秒。 一个CORSRule里面最多允许出现一个。 父节点：CORSRule

|否|
|CORSConfiguration|容器|Bucket的CORS规则容器父节点：无

|是|

## 细节分析 {#section_ixr_mvc_xdb .section}

-   默认bucket是不开启CORS功能，所有的跨域请求的origin都不被允许。
-   为了在应用程序中使用CORS功能，比如从一个www.a.com的网址通过浏览器的XMLHttpRequest功能来访问OSS，需要通过本接口手动上传CORS规则来开启。该规则由XML文档来描述。
-   每个bucket的CORS设定是由多条CORS规则指定的，每个bucket最多允许10条规则，上传的XML文档最多允许16KB大小。
-   当OSS收到一个跨域请求（或者OPTIONS请求），会读取bucket对应的CORS规则，然后进行相应的权限检查。OSS会依次检查每一条规则，使用第一条匹配的规则来允许请求并返回对应的header。如果所有规则都匹配失败则不附加任何CORS相关的header。
-   CORS规则匹配成功必须满足三个条件，首先，请求的Origin必须匹配一项AllowedOrigin项，其次，请求的方法（如GET，PUT等）或者OPTIONS请求的Access-Control-Request-Method头对应的方法必须匹配一项AllowedMethod项，最后，OPTIONS请求的Access-Control-Request-Headers头包含的每个header都必须匹配一项AllowedHeader项。
-   如果用户上传了Content-MD5请求头，OSS会计算body的Content-MD5并检查一致性，如果不一致，将返回InvalidDigest错误码。

## 示例 {#section_mlh_rvc_xdb .section}

**添加bucket跨域访问请求规则示例：**

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

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 50519080C4689A033D00235F
Date: Fri, 04 May 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

