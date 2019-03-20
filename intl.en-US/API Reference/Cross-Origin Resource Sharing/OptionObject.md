# OptionObject {#reference_jyf_j1d_xdb .reference}

Before sending a cross-domain request, the browser sends a preflight request \(OPTIONS\) containing a specified origin, HTTP method, and header information to OSS to determine whether to send a real request.

OSS can enable CORS for a bucket through PutBucketcors. After CORS is enabled for a bucket, OSS determines whether to allow the preflight request sent from the browser based on the specified CORS rules. If OSS does not allow the request or CORS is disabled for the bucket, the 403 Forbidden error is returned.

## Request syntax {#section_gpr_p1d_xdb .section}

```
OPTIONS /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Origin:Origin
Access-Control-Request-Method:HTTP method
Access-Control-Request-Headers:Request Headers
```

## Request header {#section_dxj_r1d_xdb .section}

|Header|Type|Description|
|:-----|:---|:----------|
|Origin|String|Specifies the origin of a request, which is used to identify a cross-domain request.Default value: None

|
|Access-Control-Request-Method|String|Specifies the methods to be used in a real request.Default value: None

 |
|Access-Control-Request-Headers|String|Specifies the headers \(except for simple headers\) to be used in a real request.Default value: None

|

## Response header {#section_hcv_x1d_xdb .section}

|Header|Type|Description|
|:-----|:---|:----------|
|Access-Control-Allow-Origin|String|Indicates the origin contained in the request. This header is not contained if the request is not allowed.|
|Access-Control-Allow-Methods|String|Indicates the HTTP method used by the request. This header is not contained if this request is not allowed.|
|Access-Control-Allow-Headers|String|Indicates the list of allowed headers in the request. If the request contains forbidden headers, this header is not contained and the request is rejected.|
|Access-Control-Expose-Headers|String|Indicates the list of headers that can be accessed by the client’s JavaScript application. |
|Access-Control-Max-Age|Integer|Indicates the allowed time duration \(in seconds\) required for the browser to buffer the preflight results.|

## Examples {#section_sgl_2bd_xdb .section}

Request example:

```
OPTIONS /testobject HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Origin:http://www.example.com
Access-Control-Request-Method:PUT
Access-Control-Request-Headers:x-oss-test
```

Response example:

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

