# PutBucketcors {#reference_wtg_ttc_xdb .reference}

With the PutBucketcors operation, you can set a CORS rule for a specified bucket. If an original rule exists, it is overwritten.

## Request syntax {#section_qw4_xtc_xdb .section}

```
PUT /? cors HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue

<? xml version="1.0" encoding="UTF-8"? >
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

## Request elements {#section_gvt_c5c_xdb .section}

|Name|Type|Description|Required|
|:---|:---|:----------|:-------|
|CORSRule|Container|CORS rule container. Each bucket allows up to 10 rules.Parent node: CORSConfiguration

|Yes|
|AllowedOrigin|String|The origins allowed for cross-domain requests. Multiple elements can be used to specify multiple allowed origins. Each rule allows up to one wildcard “\*“. If “\*“ is specified, cross-domain requests of all origins are allowed. Parent node: CORSRule

 |Yes|
|AllowedMethod|enumeration \(GET, PUT, DELETE, POST, HEAD\)|Specify the allowed methods for cross-domain requests.Parent node: CORSRule

|Yes|
|AllowedHeader|String|Control whether the headers specified by Access-Control-Request-Headers in the OPTIONS prefetch command are allowed. Each header specified by Access-Control-Request-Headers must match a value in AllowedHeader. Each rule allows up to one wildcard “\*” .Parent node: CORSRule

|No|
|ExposeHeader|String|Specify the response headers allowing users to access from an application \(for example, a Javascript XMLHttpRequest object\). The wildcard “\*“ is not allowed.Parent node: CORSRule

|No|
|MaxAgeSeconds|Integer|Specify the cache time for the returned result of a browser prefetch \(OPTIONS\) request to a specific resource. The unit is seconds. One CORSRule allows not more than one such parameter. Parent node: CORSRule

|No|
|CORSConfiguration|Container|CORS rule container of a bucketParent node: None

|Yes|

## Detail analysis {#section_ixr_mvc_xdb .section}

-   CORS is disabled for buckets by default. The origins of all cross-domain requests are forbidden.
-   To use CORS in applications, for example, accessing OSS from www.a.com through the XMLHttpRequest function of browser, you must manually upload a CORS rule through this interface to enable CORS. This rule is described in an XML document.
-   The CORS setting for each bucket is specified by multiple CORS rules. Each bucket allows a maximum of 10 rules. The uploaded XML document cannot be larger than 16 KB.
-   When OSS receives a cross-domain request \(or an OPTIONS request\), it reads the bucket’s CORS rules and then checks the relevant permissions. OSS checks each rule sequentially and uses the first rule that matches to approve the request and return the corresponding header. If none of the rules match, OSS does not attach any CORS header.
-   Successful CORS rule matching must satisfy three conditions. First, the request Origin must match the AllowedOrigin. Second, the request method \(for example, GET, PUT\) or the method corresponding to the Access-Control-Request-Method header in an OPTIONS request must match the AllowedMethod. Third, each header contained in the Access-Control-Request-Headers in an OPTIONS request must match the AllowedHeader.
-   If you have uploaded the Content-MD5 request header, OSS calculates the body’s Content-MD5 and check if the two are the same. If the two are different, the error code: InvalidDigest is returned.

## Example {#section_mlh_rvc_xdb .section}

**Example of adding a bucket CORS rule:**

```
PUT /? cors HTTP/1.1
                Host: oss-example.oss-cn-hangzhou.aliyuncs.com
                Content-Length: 186
                Date: Fri, 04 May 2012 03:21:12 GMT
                Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
                
                <? xml version="1.0" encoding="UTF-8"? >
                <CORSConfiguration>
                <CORSRule>
                <AllowedOrigin>*</AllowedOrigin>
                <AllowedMethod>PUT</AllowedMethod>
                <AllowedMethod>GET</AllowedMethod>
                <Alliwedheader> authorization </alliwedheader>
                </CORSRule>
                <CORSRule>
                <AllowedOrigin>http://www.a.com</AllowedOrigin>
                <AllowedOrigin>http://www.b.com</AllowedOrigin>
                <AllowedMethod>GET</AllowedMethod>
                <Alliwedheader> authorization </alliwedheader>
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

