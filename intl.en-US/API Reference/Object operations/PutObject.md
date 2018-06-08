# PutObject {#reference_l5p_ftw_tdb .reference}

The PutObject operation is used to upload files.

## Request syntax {#section_ald_lkw_bz .section}

```
PUT /ObjectName HTTP/1.1
Content-Length：ContentLength
Content-Type: ContentType
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request header {#section_y1z_lkw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|Cache-Control|String|Specifies the web page caching behavior when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt).  Default: None

|
|Content-Disposition|String|Specifies the name of the object when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt).  Default: None

|
|Content-Encoding|String|Specifies the content encoding format when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt).  Default: None

|
|Content-Md5|String|As defined in RFC 1864, the message content \(excluding the header\) is calculated to obtain an MD5 value, which is a 128-bit number. This number is encoded using Base64 into a Content-MD5 value. This request header can be used to check the validity of a message, that is, whether the message content is consistent with the sent content. This request header is optional. However, we recommend that you use this request header for an end-to-end check. Default: None

Restrictions: None

|
|Expires|String|Specifies the expiration time. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt).  Default: None

**Note:** OSS imposes no limits or verification on this value.

|
|x-oss-server-side-encryption|String|Specifies the server-side encryption algorithm when OSS creates an object. Valid values: AES256 and KMS

**Note:** You must enable the KMS \(Key Management Service\) on the console to use the KMS encryption algorithm. Otherwise, a KmsServiceNotenabled error code is reported.

|
|x-oss-object-acl|String|Specifies the access permission when OSS creates an object.  Valid values: public-read, private, and public-read-write

|

## Detail analysis {#section_fcx_vkw_bz .section}

-   If you have uploaded the Content-MD5 request header, OSS calculates Content-MD5 of the body and checks if the two are consistent. If the two are different, the error code InvalidDigest is returned.
-   If the Content-Length value in the request header is smaller than the length of data transmitted in the actual request body, OSS still creates a file, but the object size is equal to the size defined by Content-Length, and the remaining data is dropped.
-   If a file of the same name with the object to be added already exists, and you are authorized to access this object, the newly-added file overwrites the existing file, and the system returns the 200 OK message.
-   If the PutObject request carries a parameter prefixed with x-oss-meta-, the parameter is treated as user meta, for example, x-oss-meta-location. A single object can have multiple similar parameters, but the total size of all user meta cannot exceed 8 KB.
-   If the Content length parameter is not added to the header, the system returns the 411 Length Required error. Error code: MissingContentLength.
-   If the length is set, but the message body is not sent, or the size of the sent body is smaller than the specified size, the server waits until time-out, and then returns the 400 Bad Request message. Error code: RequestTimeout.
-   If the bucket of the object to be added does not exist, the system returns the 404 Not Found error. Error code: NoSuchBucket.
-   If you have no permission to access the bucket of the object to be added, the system returns the 403 Forbidden error. Error code: AccessDenied.
-   If the length of the added file exceeds 5 GB, the system returns the 400 Bad Request message. Error code: InvalidArgument.
-   If the length of the input object key exceeds 1023 bytes, the system returns the 400 Bad Request message. Error code: InvalidObjectName.
-   When you put an object, OSS supports the following five header fields defined in [RFC2616](https://www.ietf.org/rfc/rfc2616.txt): Cache-Control, Expires, Content-Encoding, Content-Disposition, and Content-Type. If these headers are set when you upload an object, the corresponding header values are automatically set to the uploaded values next time when this object is downloaded.
-   If the x-oss-server-side-encryption header is specified when you upload an object, the value of this header must be set to AES256. Otherwise, the system returns the 400 error and the error code: InvalidEncryptionAlgorithmError. After this header is specified, the response header also contains this header, and OSS stores the encryption algorithm of the uploaded object. When this object is downloaded, the response header contains x-oss-server-side-encryption, the value of which is set to the encryption algorithm of this object.

## FAQ {#section_w35_wkw_bz .section}

-   **Content-MD5 calculation method error**

    The algorithm defined  in related standards


is used to calculate Content-MD5 in the following process: Calculate the upload Content-MD5, whose value is a 128-bit binary array, and then encode the binary array with Base64. Python is used as an example. The correct calculation code is as follows:

```

>>> import base64,hashlib
>>> hash = hashlib.md5()
>>> hash.update("0123456789")
>>> base64.b64encode(hash.digest())
'eB5eJF1ptWaXm4bijSPyxw=='
```

The result of hash.digest\(\) is used as the input of Base64 encoding. `>>> hash.digest() 'x\x1e^$]i\xb5f\x97\x9b\x86\xe2\x8d#\xf2\xc7'`. A common mistake is to encode the calculated 32-byte MD5 string with Base64. Incorrect example: Encoding hash.hexdigest\(\) with Base64: `>>> hash.hexdigest() '781e5e245d69b566979b86e28d23f2c7'` The following is the incorrect calculation result. `>>> base64.b64encode(hash.hexdigest()) 'NzgxZTVlMjQ1ZDY5YjU2Njk3OWI4NmUyOGQyM2YyYzc='`

## Example {#section_orz_dlw_bz .section}

**Request example:**

```
PUT /oss.jpg HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Cache-control: no-cache
Expires: Fri, 28 Feb 2012 05:38:42 GMT
Content-Encoding: utf-8
Content-Disposition: attachment;filename=oss_download.jpg
Date: Fri, 24 Feb 2012 06:03:28 GMT
Content-Type: image/jpg
Content-Length: 344606
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk=

[344606 bytes of object data]

```

**Response example:**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Sat, 21 Nov 2015 18:52:34 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5650BD72207FB30443962F9A
x-oss-bucket-version: 1418321259
ETag: "A797938C31D59EDD08D86188F6D5B872"
```

