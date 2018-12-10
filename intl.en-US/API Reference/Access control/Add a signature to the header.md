# Add a signature to the header {#concept_aml_vv2_xdb .concept}

You can add an authorization header to carry signature information in an HTTP request to indicate that the message has been authorized.

## Calculation of the Authorization field {#section_w3s_bw2_xdb .section}

```
Authorization = "OSS " + AccessKeyId + ":" + Signature
Signature = base64(hmac-sha1(AccessKeySecret,
            VERB + "\n"
            + Content-MD5 + "\n" 
            + Content-Type + "\n" 
            + Date + "\n" 
            + CanonicalizedOSSHeaders
            + CanonicalizedResource))
```

-   The `AccessKeySecret`  indicates the key required for a signature.
-   `VERB` indicates the HTTP request method, including PUT, GET, POST, HEAD, and DELETE.
-   `\n` is a line break.
-   `Content-MD5` The Content-MD5 is the MD5 value of requested content data. The message content \(excluding the header\) is calculated to obtain an MD5 value, which is a 128-bit number. This number is encoded with Base64 into a Content-MD5 value. The request header can be used to check the message validity, that is, whether the message content is consistent with the sent content, such as “eB5eJF1ptWaXm4bijSPyxw==”. The request header may be empty. For more information, see [RFC2616 Content-MD5](https://www.ietf.org/rfc/rfc2616.txt).
-   `Content-Type` indicates the requested content type, such as “application/octet-stream”. It content type may be empty.
-   `Date` indicates the time that the operation takes. It must be in GMT format, such as “Sun, 22 Nov 2015 08:16:38 GMT”.
-   The `CanonicalizedOSSHeaders` indicates an assembly of HTTP headers whose prefixes are “x-oss-“.
-   The `CanonicalizedResource` indicates the OSS resource that the user wants to access.

Specifically, the values of Date and CanonicalizedResource cannot be empty. If the difference between the value of Date in the request and the time of the OSS server is greater than 15 minutes, the OSS server rejects the request and returns an HTTP 403 error.

## Construct CanonicalizedOSSHeaders {#section_w2k_sw2_xdb .section}

All the HTTP headers whose prefixes are x-oss- are called CanonicalizedOSSHeaders. The method to construct CanonicalizedResource is as follows:

1.  Convert the names of all HTTP request headers whose prefixes are x-oss- into lowercase letters. For example, convert `X-OSS-Meta-Name:TaoBao` to `x-oss-meta-name: TaoBao`.
2.  If the request is sent with the AccessKeyID and AccessKeySecret obtained by the STS, you must also add the obtained security-token value to the signature string in the form of `x-oss-security-token:security-token`.
3.  Sort all acquired HTTP request headers in a lexicographically ascending order.
4.  Delete any space on either side of a separator between the request header and content. For example, convert `x-oss-meta-name: TaoBao` to `x-oss-meta-name:TaoBao`.
5.  Separate all the content and headers with the `\n` separator to form the final CanonicalizedOSSHeaders.

**Note:** 

-   CanonicalizedOSSHeaders can be empty, and the `\n` at the end can be removed.
-   If only one header must be constructed, it must be `x-oss-meta-a\n`. Note the `\n` at the end.
-   If multiple headers must be constructed, it must be `x-oss-meta-a:a\nx-oss-meta-b:b\nx-oss-meta-c:c\n`. Note the `\n` at the end.

## Construct CanonicalizedResource {#section_rvv_dx2_xdb .section}

The target OSS resource specified in the request sent by the user is called a CanonicalizedResource. The method for constructing CanonicalizedResource is as follows:

1.  Set CanonicalizedResource into a null character string \(“”\);
2.  Add the OSS resource to be accessed in the following format: `/BucketName/ObjectName`. \(If ObjectName does not exist, CanonicalizedResource is “/BucketName/“. If BucketName does not exist either, CanonicalizedResource is “/“.\)
3.  If the requested resource includes sub-resources \(SubResource\), sort all the sub-resources in a lexicographically ascending order and separate the sub-resources using the separator `&` to generate a sub-resource string. Add “?” and the sub-resource string to the end of the CanonicalizedResource string. In this case, CanonicalizedResource is like: `/BucketName/ObjectName?acl&uploadId=UploadId`
4.  If the user request specifies the query string \(QueryString, also called HTTP Request Parameters\), sort these query strings and request values in a lexicographically ascending order, separate the query strings and request values using the separator `&`, and add them to CanonicalizedResource based on the parameters. In this case, CanonicalizedResource is like:  `/BucketName/ObjectName?acl&response-content-type=ContentType&uploadId=UploadId`.

**Note:** 

-   The sub-resources supported by OSS currently include: acl, uploads, location, cors, logging, website, referer, lifecycle, delete, append, tagging, objectMeta, uploadId, partNumber, security-token, position, img, style, styleName, replication, replicationProgress, replicationLocation, cname, bucketInfo, comp, qos, live, status, vod, startTime, endTime, symlink, x-oss-process, response-content-type, response-content-language, response-expires, response-cache-control, response-content-disposition, and response-content-encoding.
-   Three types of sub-resources are available:
    -   Resource identifiers, such as acl, append, uploadId, and symlink sub-resources. For more information, see [Bucket-related operations](reseller.en-US/API Reference/Bucket operations/PutBucket.md#) and [Object-related operations](reseller.en-US/API Reference/Object operations/PutObject.md#).
    -   Specify response header fields such as `response-***`. For more information, see the `Request Parameters section` of [GetObject](reseller.en-US/API Reference/Object operations/GetObject.md#) .
    -   Object handling methods, such as `x-oss-process`. It is used as the object handling method, such as [Image Processing](../../../../reseller.en-US/Image Processing Guide/Image processing access rules.md#).

## Rules to calculate a signature header {#section_qcb_p1f_xdb .section}

-   A signature string must be in the UTF-8 format. Encode a signature string containing Chinese characters with UTF-8 first, and then use it with the AccessKeySecret to calculate the final signature.
-   The signing method adopted is the HMAC-SHA1 method defined in [RFC 2104](http://www.ietf.org/rfc/rfc2104.txt), where Key is`AccessKeySecret` .
-   Content-Type and Content-MD5 are not required in a request. If the request requires signature verification, the null value can be replaced with the line break “\\n”.
-   Among all non-HTTP-standard headers, only the headers starting with “x-oss-“ require signature strings,  and other non-HTTP-standard headers are ignored by OSS. \(For example, the “x-oss-magic” header in the preceding example must be added with a signature string.\)
-   Headers starting with “x-oss-“ must comply with the following specifications before being used for signature verification:
    -   The header name is changed to lower-case letters.
    -   The headers are sorted in a lexicographically ascending order.
    -   No space exists before and after the colon, which separates the header name and value.
    -   Each header is followed by the line break “\\n”. If no header is used, CanonicalizedOSSHeaders is set to null.

## Example signature {#section_qxg_s1f_xdb .section}

Assume that AccessKeyID is 44CF9590006BF252F707 and AccessKeySecret is OtxrzxIsfpFjA7SwPzILwy8Bw21TLhquhboDYROV.

|Request|Signature string calculation formula|Signature string|
|:------|:-----------------------------------|:---------------|
|PUT /nelson HTTP/1.0 Content-MD5: eB5eJF1ptWaXm4bijSPyxw== Content-Type: text/html Date: Thu, 17 Nov 2005 18:49:58 GMT Host: oss-example.oss-cn-hangzhou.aliyuncs.com X-OSS-Meta-Author: foo@bar.com X-OSS-Magic: abracadabra|Signature = base64\(hmac-sha1\(AccessKeySecret,VERB + “\\n” + Content-MD5 + “\\n”+ Content-Type + “\\n” + Date + “\\n” + CanonicalizedOSSHeaders+ CanonicalizedResource\)\)|“PUT\\n eB5eJF1ptWaXm4bijSPyxw==\\n text/html\\n Thu, 17 Nov 2005 18:49:58 GMT\\n x-oss-magic:abracadabra\\nx-oss-meta-author:foo@bar.com\\n/oss-example/nels|

The signature calculation method is as follows:

Python sample code:

```
import base64
import hmac
import sha
h = hmac.new("OtxrzxIsfpFjA7SwPzILwy8Bw21TLhquhboDYROV",
             "PUT\nODBGOERFMDMzQTczRUY3NUE3NzA5QzdFNUYzMDQxNEM=\ntext/html\nThu, 17 Nov 2005 18:49:58 GMT\nx-oss-magic:abracadabra\nx-oss-meta-author:foo@bar.com\n/oss-example/nelson", sha)
Signature = base64.b64encode(h.digest())
print("Signature: %s" % Signature)
```

The signature calculation result is 26NBxoKdsyly4EDv6inkoDft/yA=. According to the formula Authorization = “OSS “ + AccessKeyID + “:” + Signature, the value of Authorization is OSS 44CF9590006BF252F707:26NBxoKdsyly4EDv6inkoDft/yA=. The value is added with the authorization header to form the message to be sent:

```
PUT /nelson HTTP/1.0
Authorization:OSS 44CF9590006BF252F707:26NBxoKdsyly4EDv6inkoDft/yA=
Content-Md5: eB5eJF1ptWaXm4bijSPyxw==
Content-Type: text/html
Date: Thu, 17 Nov 2005 18:49:58 GMT
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
X-OSS-Meta-Author: foo@bar.com
X-OSS-Magic: abracadabra
```

## Detail analysis {#section_dbw_1bf_xdb .section}

-   If the input AccessKeyID does not exist or is inactive, the error 403 Forbidden is returned. Error code: InvalidAccessKeyId.
-   If the authorization value format in the user request header is incorrect, the error 400 Bad Request is returned.  Error code: InvalidArgument.
-   All the requests of OSS must use the GMT time format stipulated by the HTTP 1.1 protocol. Specifically, the date format is:`date1 = 2DIGIT SP month SP 4DIGIT; day month year(for example, 02 Jun 1982)`. In the aforesaid date format, “day” occupies “2 digits”. Therefore,  “Jun 2”, “2 Jun 1982”, and “2-Jun-82” are all invalid date formats.
-   If Date is not input into the header or the format is incorrect during signature verification, the error 403 Forbidden is returned. Error code: AccessDenied.
-   The request must be entered within 15 minutes based on the current time of the OSS server; otherwise, the error 403 Forbidden is returned. Error code: RequestTimeTooSkewed.
-   If the AccessKeyID is active but OSS determines that the signature of the user request is incorrect, the error 403 Forbidden is returned, and the correct signature string for verification and encryption is returned to the user in the response message. The user can check whether or not the signature string is correct based on the response of OSS. Return example:

    ```
    <? xml version="1.0" ? >
    <Error>
     <Code>
         SignatureDoesNotMatch
     </Code>
     <Message>
         The request signature we calculated does not match the signature you provided. Check your key and signing method.
     </Message>
     <StringToSignBytes>
         47 45 54 0a 0a 0a 57 65 64 2c 20 31 31 20 4d 61 79 20 32 30 31 31 20 30 37 3a 35 39 3a 32 35 20 47 4d 54 0a 2f 75 73 72 65 61 6c 74 65 73 74 3f 61 63 6c
     </StringToSignBytes>
     <RequestId>
         1E446260FF9B10C2
     </RequestId>
     <HostId>
         oss-cn-hangzhou.aliyuncs.com
     </HostId>
     <SignatureProvided>
         y5H7yzPsA/tP4+0tH1HHvPEwUv8=
     </SignatureProvided>
     <StringToSign>
         GET
    Wed, 11 May 2011 07:59:25 GMT
    /oss-example? acl
     </StringToSign>
     <OSSAccessKeyId>
         AKIAIVAKMSMOY7VOMRWQ
     </OSSAccessKeyId>
    </Error>
    ```


**Note:** OSS SDK has implemented the signature. You do not need to worry about the signature issue when you use the OSS SDK. To learn more about the signature implementations of specific languages, see the OSS SDK code. The files for implementing OSS SDK signature are shown in the following table:

|SDK|Signature implementation|
|:--|:-----------------------|
|Java SDK|[OSSRequestSigner.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/main/java/com/aliyun/oss/internal/OSSRequestSigner.java)|
|Python SDK|[auth.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/oss2/auth.py)|
|Net SDK|[OssRequestSigner.cs](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/sdk/Util/OssRequestSigner.cs)|
|PHP SDK|[OssClient.php](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/src/OSS/OssClient.php)|
|C SDK|[oss\_auth.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk/oss_auth.c)|
|JavaScript SDK|[client.js](https://github.com/ali-sdk/ali-oss/blob/master/lib/client.js)|
|Go SDK|[auth.go](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/oss/auth.go)|
|Ruby SDK|[util.rb](https://github.com/aliyun/aliyun-oss-ruby-sdk/blob/master/lib/aliyun/oss/util.rb)|
|iOS SDK|[OSSModel.m](https://github.com/aliyun/aliyun-oss-ios-sdk/blob/master/AliyunOSSSDK/OSSModel.m)|
|Android SDK|[OSSUtils.java](https://github.com/aliyun/aliyun-oss-android-sdk/blob/master/oss-android-sdk/src/main/java/com/alibaba/sdk/android/oss/common/utils/OSSUtils.java)|

## Content-MD5 calculation method {#section_vkz_sbf_xdb .section}

```
Content-MD5 calculation
The message content "123456789" is used as an example. The Content-MD5 value of the string
is calculated as follows:
The algorithm defined in related standards can be simplified to the following:
Calculate the MD5-encrypted 128-bit binary array.
Encode the binary array (instead of the 32-bit string code) with Base64.  
Python is used as an example.
The correct calculation code is: 
>>> import base64,hashlib
>>> hash = hashlib.md5()
>>> hash.update("0123456789")
>>> base64.b64encode(hash.digest())
'eB5eJF1ptWaXm4bijSPyxw=='
Note:
The correct code is: hash.digest(), used to calculate a 128-bit binary array
>>> hash.digest()
'x\x1e^$]i\xb5f\x97\x9b\x86\xe2\x8d#\xf2\xc7'
The common error is to base 64 the computed 32-Bit String encoding directly.
An incorrect example: hash.hexdigest(), and a visible 32-bit string is calculated.
>>> hash.hexdigest()
'781e5e245d69b566979b86e28d23f2c7'
Result of encoding the incorrect MD5 value with Base64: 
>>> base64.b64encode(hash.hexdigest())
'NzgxZTVlMjQ1ZDY5YjU2Njk3OWI4NmUyOGQyM2YyYzc='
```

