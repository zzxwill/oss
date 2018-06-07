# GetBucketcors {#reference_m2w_ywc_xdb .reference}

GetBucketcors用于获取指定的Bucket目前的CORS规则。

## 请求语法 {#section_nvn_gxc_xdb .section}

```
GET /?cors HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素\(Response Elements\) {#section_j24_3xc_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|CORSRule|容器|CORS规则的容器，每个bucket最多允许10条规则。 父节点：CORSConfiguration

|
|AllowedOrigin|字符串|指定允许的跨域请求的来源，允许使用多个元素来指定多个允许的来源。 允许使用最多一个“\*”通配符。如果指定为“\*”则表示允许所有的来源的跨域请求。父节点：CORSRule

|
|AllowedMethod|枚举（GET,PUT,DELETE,POST,HEAD）|指定允许的跨域请求方法。父节点：CORSRule

|
|AllowedHeader|字符串|控制在OPTIONS预取指令中Access-Control-Request-Headers头中指定的header是否允许。在Access-Control-Request-Headers中指定的每个header都必须在AllowedHeader中有一条对应的项。允许使用最多一个“\*”通配符。 父节点：CORSRule

|
|ExposeHeader|字符串|指定允许用户从应用程序中访问的响应头（例如一个Javascript的XMLHttpRequest对象。不允许使用“\*”通配符。父节点：CORSRule

 |
|MaxAgeSeconds|整型|指定浏览器对特定资源的预取（OPTIONS）请求返回结果的缓存时间，单位为秒。 一个CORSRule里面最多允许出现一个。父节点：CORSRule

 |
|CORSConfiguration|容器|Bucket的CORS规则容器 父节点：无

 |

## 细节分析 {#section_g1k_byc_xdb .section}

-   如果Bucket不存在，返回404 no content错误。错误码：NoSuchBucket。
-   只有Bucket的拥有者才能获取CORS规则，否则返回403 Forbidden错误,错误码：AccessDenied。
-   如果CORS规则不存在，返回404 Not Found错误，错误码NoSuchCORSConfiguration。

## 示例 {#section_c3z_dyc_xdb .section}

**请求示例：**

```
Get /?cors HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Thu, 13 Sep 2012 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=
```

**已设置CORS规则的返回示例：**

```
HTTP/1.1 200
x-oss-request-id: 50519080C4689A033D00235F
Date: Thu, 13 Sep 2012 07:51:28 GMT
Connection: keep-alive
Content-Length: 218  
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration>
    <CORSRule>
      <AllowedOrigin>*</AllowedOrigin>
      <AllowedMethod>GET</AllowedMethod>
      <AllowedHeader>*</AllowedHeader>
      <ExposeHeader>x-oss-test</ExposeHeader>
      <MaxAgeSeconds>100</MaxAgeSeconds>
    </CORSRule>
</CORSConfiguration>
```

