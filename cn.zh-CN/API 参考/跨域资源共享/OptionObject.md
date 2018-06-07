# OptionObject {#reference_jyf_j1d_xdb .reference}

浏览器在发送跨域请求之前会发送一个preflight请求（OPTIONS）并带上特定的来源域，HTTP方法和header信息等给OSS以决定是否发送真正的请求。

OSS可以通过Put Bucket cors接口来开启Bucket的CORS支持，开启CORS功能之后，OSS在收到浏览器preflight请求时会根据设定的规则评估是否允许本次请求。如果不允许或者CORS功能没有开启，返回403 Forbidden。

## 请求语法 {#section_gpr_p1d_xdb .section}

```
OPTIONS /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Origin:Origin
Access-Control-Request-Method:HTTP method
Access-Control-Request-Headers:Request Headers
```

## 请求Header {#section_dxj_r1d_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|Origin|字符串|请求来源域，用来标示跨域请求。默认值：无

|
|Access-Control-Request-Method|字符串|表示在实际请求中将会用到的方法。默认值：无

 |
|Access-Control-Request-Headers|字符串|表示在实际请求中会用到的除了简单头部之外的headers。 默认值：无

|

## 响应Header {#section_hcv_x1d_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|Access-Control-Allow-Origin|字符串|请求中包含的Origin，如果不允许的话将不包含该头部。|
|Access-Control-Allow-Methods|字符串|允许请求的HTTP方法，如果不允许该请求，则不包含该头部。|
|Access-Control-Allow-Headers|字符串|允许请求携带的header的列表，如果请求中有不被允许的header，则不包含该头部，请求也将被拒绝。|
|Access-Control-Expose-Headers|字符串|允许在客户端JavaScript程序中访问的headers的列表。|
|Access-Control-Max-Age|整型|允许浏览器缓存preflight结果的时间，单位为秒|

## 示例 {#section_sgl_2bd_xdb .section}

**请求示例：**

```
OPTIONS /testobject HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Origin:http://www.example.com
Access-Control-Request-Method:PUT
Access-Control-Request-Headers:x-oss-test
```

**返回示例：**

```
HTTP/1.1 200 OK 
x-oss-request-id: 5051845BC4689A033D0022BC
Date: Fri, 24 Feb 2012 05:45:34 GMT
Access-Control-Allow-Origin: http://www.example.com
Access-Control-Allow-Methods: PUT
Access-Control-Expose-Headers: x-oss-test
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

