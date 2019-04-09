# GetBucketCORS {#reference_m2w_ywc_xdb .reference}

GetBucketCORS接口用于获取指定存储空间（Bucket）当前的跨域资源共享（CORS）规则。

## 请求语法 {#section_nvn_gxc_xdb .section}

```
GET /?cors HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素 {#section_j24_3xc_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|CORSRule|容器| CORS规则的容器。每个Bucket最多允许10条规则。

 父节点：CORSConfiguration

 |
|AllowedOrigin|字符串| 允许的跨域请求来源。 星号（\*）表示允许所有的来源的跨域请求。

 父节点：CORSRule

 |
|AllowedMethod|枚举（GET,PUT,DELETE,POST,HEAD）| 允许的跨域请求方法。

 父节点：CORSRule

 |
|AllowedHeader|字符串| 控制在OPTIONS预取指令中Access-Control-Request-Headers指定的header是否允许。在Access-Control-Request-Headers中指定的每个header都在AllowedHeader中有一条对应的项。

 父节点：CORSRule

 |
|ExposeHeader|字符串| 允许用户从应用程序中访问的响应头（例如一个Javascript的XMLHttpRequest对象）。

 父节点：CORSRule

 |
|MaxAgeSeconds|整型| 浏览器对特定资源的预取（OPTIONS）请求返回结果的缓存时间。 一个CORS规则里面最多允许出现一个MaxAgeSeconds。

 单位：秒

 父节点：CORSRule

 |
|CORSConfiguration|容器| Bucket的CORS规则容器。

 父节点：无

 |

## 示例 {#section_c3z_dyc_xdb .section}

**请求示例**

```
Get /?cors HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Thu, 13 Sep 2012 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=
```

**返回示例**

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
|NoSuchBucket|404|目标Bucket不存在。|
|NoSuchCORSConfiguration|404|目标CORS规则不存在。|
|AccessDenied|403|只有Bucket的拥有者才能获取CORS规则。|

