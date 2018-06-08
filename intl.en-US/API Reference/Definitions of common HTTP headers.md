# Definitions of common HTTP headers {#reference_mhp_zdy_wdb .reference}

## Common request headers {#section_eq1_b2y_wdb .section}

Some common request headers are used in the OSS RESTful interfaces. These request headers can be used by all the OSS requests. The following table lists the specific definitions of the request headers:

|Name|Type|Description|
|:---|:---|:----------|
|Authorization|string|The verification information used to verify the validity of a request.Default value: none

Usage scenario: non-anonymous requests

|
|Content-Length|string|Content length of an HTTP request, which is defined in [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: none

Usage scenario: requests that need to submit data to OSS

|
|Content-Type|string|Content type of an HTTP request, which is defined in [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: none

Usage scenario: requests that need to submit data to OSS

|
|date|string|The GMT time stipulated in the HTTP 1.1 protocol, for example, Wed, 05 Sep. 2012 23:00:00 GMT Default value: none

|
|Host|string|The access host value. Format: `<bucketname>.oss-cn-hangzhou.aliyuncs.com`. Default value: none

|

## Common response headers {#section_hcd_m2y_wdb .section}

Some common response headers are used in the OSS RESTful interfaces. These response headers can be used by all the OSS requests. The following table lists the specific definitions of the response headers:

|Name|Type|Description|
|:---|:---|:----------|
|Content-Length|string|Content length of an HTTP request, which is defined in [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: none

Usage scenario: requests that need to submit data to OSS

|
|Connection|enumerative|The connection status between the client and the OSS server.Valid values: "open" and "close”

Default value: none

|
|Date|string|The GMT time stipulated in the HTTP 1.1 protocol, for example, Wed, 05 Sep. 2012 23:00:00 GMTDefault value: none

 |
|Etag|string|The ETag \(entity tag\) is created when an object is generated and is used to indicate the content of the object. For an object created for a Put Object request, the value of ETag is the value of MD5 in the content of the object. For an object created in other approaches, the value of ETag is the UUID in the content of the object. The value of ETag can be used to check whether the content of the object is changed.Default value: none

|
|Server|string|The server that generates the response. Default value: AliyunOSS

|
|x-oss-request-id|string|The UUID of the response. It is created by Alibaba Cloud OSS. In case of any issues when using the OSS service, you can contact OSS support personnel using this field to rapidly locate the issue.Default value: none

|

