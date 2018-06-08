# OptionObject {#reference_jyf_j1d_xdb .reference}

Before sending a cross-domain request, the browser sends a preflight request \(OPTIONS\) containing a specific origin, HTTP method, and header information to OSS to determine whether to send a real request.

OSS can enable CORS for a bucket through the Put Bucket cors interface. After CORS is enabled, OSS assesses whether to allow the preflight request of the browser based on the specified rules. If OSS does not allow this request or CORS is disabled, the error 403 Forbidden is returned.

## Request syntax {#section_gpr_p1d_xdb .section}

```
OPTIONS /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Origin:Origin
Access-Control-Request-Method:HTTP method
Access-Control-Request-Headers:Request Headers
```

## Request header {#section_dxj_r1d_xdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|Origin|String|Origin of a request, used to identify a cross-domain request. Default: None

|
|Access-Control-Request-Method|String|Methods to be used in an actual request.Default: None

 |
|Access-Control-Request-Headers|String|Headers, except simple headers, to be used in an actual request.  Default: None

|

## Response header {#section_hcv_x1d_xdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|Access-Control-Allow-Origin|String|Origin contained in a request. This header is not contained if this request is not allowed. |
|Access-Control-Allow-Methods|String|HTTP method used by a request. This header is not contained if this request is not allowed.|
|Access-Control-Allow-Headers|String|Header list carried in a request. If the request contains forbidden headers, this header is not contained and the request is rejected.|
|Access-Control-Expose-Headers|String|Header list that can be accessed by the client’s JavaScript application. |
|Access-Control-Max-Age|Integer|Time duration when the browser can buffer the preflight results. The unit is seconds.|

## Example {#section_sgl_2bd_xdb .section}

**Request example:**

```
OPTIONS /testobject HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Origin:http://www.example.com
Access-Control-Request-Method:PUT
Access-Control-Request-Headers:x-oss-test
```

**Response example:**

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

