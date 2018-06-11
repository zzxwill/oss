# Add a signature to a URL {#concept_xqh_2df_xdb .concept}

In addition to using an authorization header, you can add signature information to a URL. It enables you to forward a URL to the third party for an authorized access.

## Implementation {#section_rtl_3df_xdb .section}

URL signature example:

```
http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936**9l&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv4%3D
```

The URL signature must include at least the following three parameters: Signature, Expires, and OSSAccessKeyId.

-   The `Expires`  parameter indicates the time-out period of a URL. The value of this parameter is UNIX time \(which is the number of seconds that have elapsed since 00:00:00 UTC, January 1, 1970. For more information, see [Wikipedia](https://en.wikipedia.org/wiki/Unix_time)\).  If the time when OSS receives the URL request is later than the value of the Expires parameter and is included in the signature, an error code request timed-out is returned. For example, if the current time is 1141889060, to create a URL that is scheduled to expire in 60 seconds, you can set the value of Expires to 1141889120.
-   `OSSAccessKeyId` refers to the AccessKeyID in the key.
-   `Signature`  indicates the signature information. For all requests and header parameters that OSS supports, the algorithm for adding a signature to a URL is basically the same as that of [Adding a signature to a header](intl.en-US/API Reference/Access control/Add a signature to the header.md#).

    ```
    Signature = urlencode(base64(hmac-sha1(AccessKeySecret,
              VERB + "\n" 
              + CONTENT-MD5 + "\n" 
              + CONTENT-TYPE + "\n" 
              + EXPIRES + "\n" 
              + CanonicalizedOSSHeaders
              + CanonicalizedResource)))
    ```

    The difference is listed as follows:

    -   When a signature is added to a URL, the Expires parameter replaces the Date parameter.
    -   Signatures cannot be included in a URL and the Header at the same time.
    -   If more than one incoming Signature, Expires, or AccessKeyId value is available, the first of each incoming value is used.
    -   Whether the request time is later than the Expires time, is verified first before verifying the signature.
    -   When you put the signature string into a URL, remember to perform the UrlEncode for a URL.
-   When you add a signature to a temporary user URL, the `security-token` must also be entered. The format is as follows:

    ```
    http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936**9l&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv4%3D&security-token=SecurityToken
    ```


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

**Note:** 

-   The preceding code is the Python sample code.
-   OSS SDK provides the method for adding a signature into an URL. For usage, see the “Authorize Access” section in the SDK file.
-   To add a signature to the OSS SDK URL, see the following table.

    |SDK|URL signature method|Implementation file|
    |:--|:-------------------|:------------------|
    |Java SDK|OSSClient.generatePresignedUrl|[OSSClient.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/main/java/com/aliyun/oss/OSSClient.java?spm=a2c4g.11186623.2.6.30uUQV&file=OSSClient.java)|
    |Python SDK|Bucket.sign\_url|[api.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/oss2/api.py?spm=a2c4g.11186623.2.7.30uUQV&file=api.py)|
    |Net SDK|OssClient.GeneratePresignedUri|[OssClient.cs](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/sdk/OssClient.cs?spm=a2c4g.11186623.2.8.30uUQV&file=OssClient.cs)|
    |PHP SDK|OssClient.signUrl|[OssClient.php](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/src/OSS/OssClient.php?spm=a2c4g.11186623.2.9.30uUQV)|
    |JavaScript SDK|signatureUrl|[object.js](https://github.com/ali-sdk/ali-oss/blob/master/lib/object.js?spm=a2c4g.11186623.2.10.30uUQV&file=object.js)|
    |C SDK|oss\_gen\_signed\_url|[oss\_object.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk/oss_object.c?spm=a2c4g.11186623.2.11.30uUQV&file=oss_object.c)|


## Detail analysis {#section_cbj_q2f_xdb .section}

-   If you adopt the approach of adding a signature to a URL, the authorized data is exposed on the Internet before the authorization period expires. We recommend that you must assess the usage risks in advance.
-   The PUT and GET requests both support adding a signature in a URL.
-   When a signature is added to a URL, the sequence of Signature, Expires, and AccessKeyId can be swapped. If one or more Signature, Expires, or AccessKeyId parameter is missing, the error 403 Forbidden is returned. Error code: AccessDenied.
-   If the current access time is later than the Expires time set in the request, the error 403 Forbidden is returned. Error code: AccessDenied.
-   If the format of the Expires time is incorrect, the error 403 Forbidden is returned. Error code: AccessDenied.
-   If the URL includes one or more Signature, Expires, or AccessKeyId parameter and the header also includes signature information, the error 400 Bad Request is returned. Error code: InvalidArgument.
-   When the signature string is generated, the Date parameter is replaced by the Expires parameter, but the headers such as content-type and content-md5 defined in the preceding section are still included. \(Though the Date request header still exists in the request, you can skip adding it to the signature string.\)

