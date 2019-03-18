# Add a signature to a URL {#concept_xqh_2df_xdb .concept}

In addition to using an authorization header, you can also add signature information to a URL so that you can forward the URL to the third party for authorized access.

## Sample code {#section_wcr_k2f_xdb .section}

Python sample code used to add a signature to a URL:

```
import base64
import hmac
import sha
import urllib
h = hmac.new("OtxrzxIsfpFjA7SwPzILwy8Bw21TLhquhboDYROV",
             "GET\n\n\n1141889120\n/oss-example/oss-api.pdf",
             sha)
urllib.quote (base64.encodestring(h.digest()).strip())
```

OSS SDK provides the method for adding a signature into an URL. For the detailed usage, see Authorized access in OSS SDK documentation.

To add a signature to the OSS SDK URL, see the following table.

|SDK|URL signature method|Implementation file|
|:--|:-------------------|:------------------|
|Java SDK|OSSClient.generatePresignedUrl|[OSSClient.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/main/java/com/aliyun/oss/OSSClient.java?spm=a2c4g.11186623.2.6.30uUQV&file=OSSClient.java)|
|Python SDK|Bucket.sign\_url|[api.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/oss2/api.py?spm=a2c4g.11186623.2.7.30uUQV&file=api.py)|
|Net SDK|OssClient.GeneratePresignedUri|[OssClient.cs](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/sdk/OssClient.cs?spm=a2c4g.11186623.2.8.30uUQV&file=OssClient.cs)|
|PHP SDK|OssClient.signUrl|[OssClient.php](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/src/OSS/OssClient.php?spm=a2c4g.11186623.2.9.30uUQV)|
|JavaScript SDK|signatureUrl|[object.js](https://github.com/ali-sdk/ali-oss/blob/master/lib/object.js?spm=a2c4g.11186623.2.10.30uUQV&file=object.js)|
|C SDK|oss\_gen\_signed\_url|[oss\_object.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk/oss_object.c?spm=a2c4g.11186623.2.11.30uUQV&file=oss_object.c)|

## Implementation {#section_rtl_3df_xdb .section}

URL signature example:

```
http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936**9l&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv4%3D
```

The URL signature must include at least the following three parameters: Signature, Expires, and OSSAccessKeyId.

-   The `Expires` parameter indicates the timeout period of a URL. The value of this parameter is [UNIX time](https://en.wikipedia.org/wiki/Unix_time) \(which is the number of seconds that have elapsed since 00:00:00 UTC, January 1, 1970\). If the time when OSS receives the URL request is later than the value of the Expires parameter included in the signature, an error code of request timed-out is returned. For example, if the current time is 1141889060, to create a URL that is scheduled to expire in 60 seconds, you can set the value of Expires to 1141889120. The valid period of a URL is 3,600 seconds by default and 64,800 seconds in maximum.
-   `OSSAccessKeyId` refers to the AccessKeyID in the key.
-   `Signature` indicates the signature information. For all requests and header parameters that OSS supports, the algorithm for adding a signature to a URL is basically the same as that of [Adding a signature to a header](intl.en-US/API Reference/Access control/Add a signature to the header.md#).

    ```
    Signature = urlencode(base64(hmac-sha1(AccessKeySecret,
              VERB + "\n" 
              + CONTENT-MD5 + "\n" 
              + CONTENT-TYPE + "\n" 
              + EXPIRES + "\n" 
              + CanonicalizedOSSHeaders
              + CanonicalizedResource)))
    ```

    The differences are listed as follows:

    -   When a signature is added to a URL, the Date parameter is replaced by the Expires parameter.
    -   Signatures cannot be included in a URL and the Header at the same time.
    -   If the value of Signature, Expires, or AccessKeyId is passed in for multiple times, the value passed for the first time is used.
    -   Before the signature is verified, the request time is verified to check whether it is later than the value of Expires.
    -   Before adding the signature string into a URL, perform the UrlEncode for the URL.
-   When you add the signature to a URL as a temporary user, the `security-token` must also be included. The format is as follows:

    ```
    http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936**9l&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv4%3D&security-token=SecurityToken
    ```


## Detail analysis {#section_cbj_q2f_xdb .section}

-   If you add a signature to a URL, the authorized data is exposed on the Internet before the authorization period expires. We recommend that you assess the risks in advance.
-   PUT and GET requests support adding a signature in a URL.
-   When a signature is added to a URL, the sequence of Signature, Expires, and AccessKeyId can be swapped. However, if one or more of the Signature, Expires, or AccessKeyId parameter is missing, the 403 Forbidden error message is returned with the error code: AccessDenied.
-   If the current access time is later than the value of Expires set in the request or the format of Expires is incorrect, the 403 Forbidden error message is returned with the error code: AccessDenied.
-   If the URL includes one or more of the Signature, Expires, or AccessKeyId parameter and the header also includes signature information, the 400 Bad Request error message is returned with the error code: InvalidArgument.
-   When the signature string is generated, the Date parameter is replaced by the Expires parameter, but the headers defined in the preceding section, such as content-type and content-md5, are still included. \(The Date header is still included in the request, but it does not need to be added into the signature string.\)

