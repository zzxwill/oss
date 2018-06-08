# GetBucketcors {#reference_m2w_ywc_xdb .reference}

The GetBucketcors operation is used to obtain the current CORS rules of a specified bucket.

## Request syntax {#section_nvn_gxc_xdb .section}

```
GET /? cors HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_j24_3xc_xdb .section}

|Name|Type |Description |
|:---|:----|:-----------|
|CORSRule|Container |CORS rule container. Each bucket allows up to 10 rules. Parent node: CORSConfiguration

|
|AllowedOrigin|String|The origins allowed for cross-domain requests. Multiple elements can be used to specify multiple allowed origins. Each rule allows up to one wildcard “\*“. If “\*“ is specified, cross-domain requests of all origins are allowed.Parent node: CORSRule

|
|AllowedMethod|enumeration \(GET, PUT, DELETE, POST, HEAD\)|Specify the allowed methods for cross-domain requests.Parent node: CORSRule

|
|AllowedHeader|String|Control whether the headers specified by Access-Control-Request-Headers in the OPTIONS prefetch command are allowed. Each header specified by Access-Control-Request-Headers must match a value in AllowedHeader. Each rule allows up to one wildcard “\*”  Parent node: CORSRule

|
|ExposeHeader|String|Specify the response headers allowing users to access from an application \(for example, a Javascript XMLHttpRequest object\). The wildcard “\*“ is not allowed. Parent node: CORSRule

 |
|MaxAgeSeconds|Integer|Specify the cache time for the returned result of a browser prefetch \(OPTIONS\) request to a specific resource. The unit is seconds. One CORSRule allows not more than one such parameter. Parent node: CORSRule

 |
|CORSConfiguration|Container|CORS rule container of a bucket Parent node: None

 |

## Detail analysis  {#section_g1k_byc_xdb .section}

-   If a bucket does not exist, the error “404 no content” is returned. Error code: NoSuchBucket.
-   Only the bucket owner can obtain CORS rules. Otherwise, the error 403 Forbidden is returned with the error code: AccessDenied.
-   If CORS rules do not exist, OSS returns the “404 Not Found” error with the error code: NoSuchCORSConfiguration.

## Example {#section_c3z_dyc_xdb .section}

**Request example:**

```
GET /? cors HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Thu, 13 Sep 2012 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=
```

**Response example with CORS rules already set:**

```
HTTP/1.1 200
x-oss-request-id: 50519080C4689A033D00235F
Date: Thu, 13 Sep 2012 07:51:28 GMT
Connection: keep-alive
Content-Length: 218  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
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

