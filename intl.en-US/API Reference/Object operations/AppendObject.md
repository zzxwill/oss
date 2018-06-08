# AppendObject {#reference_fvf_xld_5db .reference}

AppendObject is used to upload files in appending mode.

The type of the objects created with the Append Object operation is Appendable Object, and the type of the objects uploaded with the Put Object operation is Normal Object.

## Request syntax {#section_n23_kpw_bz .section}

```
POST /ObjectName? append&position=Position HTTP/1.1
Content-Length：ContentLength
Content-Type: ContentType
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request header {#section_x4j_lpw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|Cache-Control|String|Specifies the cache action of the web page when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt).  Default: None

|
|Content-Disposition|String|Specifies the name of the object downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt).  Default: None

|
|Content-Encoding|String|Specifies the content encoding format when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default: None

|
|Content-MD5|String|As defined in RFC  1864, the message content \(excluding the header\) is computed to obtain an MD5 value, which is a 128-bit number. Then, this number is Base64-encoded into a Content-MD5 value. This request header can be used for checking the validity of a message, that is, whether the message content is consistent with the sent content. Although this request header is optional, we recommend that you use this request header for end-to-end check.  Default: None

Restrictions: None

|
|Expires|Integer|Indicates the expiration time. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default: None

|
|x-oss-server-side-encryption|String|Specifies the server-side encryption algorithm when OSS creates an object.  Valid value: AES256 或 KMS

**Note:** You must enable the KMS \(Key Management Service\) on the console to use the KMS encryption algorithm. Otherwise, a KmsServiceNotenabled error code is reported.

|
|x-oss-object-acl|String|Specifies the access permission when OSS creates an object.  Valid values: public-read，private，public-read-write

|

## Response header {#section_xbv_rpw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|x-oss-next-append-position|64-bit integer|Specifies the position that must be provided in the next request. It is in fact the current object length. This header is contained when a successful message is returned for Append Object, or when a 409 error occurs because of a position mismatch and the object length.|
|x-oss-hash-crc64ecma|64-bit integer|Specifies the 64-bit CRC value of the object.  The 64-bit CRC value is calculated according to [ECMA-182 Standard](http://www.ecma-international.org/publications/standards/Ecma-182.htm).|

## Association with other operations {#section_vgd_wpw_bz .section}

-   Append Object is not applicable to a non-appendable object. For example, if a normal object with the same name already exists and the Append Object operation is still performed, the system returns the 409 message and the error code ObjectNotAppendable.
-   If you perform the Put Object operation on an existing appendable object, this appendable object is overwritten by the new object, and the type of this object is changed to Normal Object.
-   After the Head Object operation is performed, the system returns x-oss-object-type, which indicates the type of the object. If the object is an appendable object, the value of x-oss-object-type is Appendable. For an appendable object, after the Head Object operation is performed, the system also returns x-oss-next-append-position and x-oss-hash-crc64ecma.
-   In the response XML of the Get Bucket \(List Objects\) request, the type of an appendable object is set to Appendable.
-   You can neither use Copy Object to copy an appendable object, nor change the server-side encryption attribute of this object. You can use Copy Object to modify the custom metadata.

## Detail analysis {#section_jxx_wpw_bz .section}

-   The two URL parameters, append, and position, are both CanonicalizedResource, and must be contained in the signature.
-   URL parameters must also contain append, which specifies that the operation is an Append Object operation.
-   URL query parameters must contain position, which specifies the position from where appending starts. The value of position in the first Append Object operation must be 0, and the value of position in the subsequent operation is the current object length. For example, if the value of position specified in the first Append Object request is 0, and the value of content-length is 65536, the value of position specified in the second Append Object request must be set to 65536. After each operation succeeds, x-oss-next-append-position in the response header also specifies the position of the next Appendix Object request.
-   If the position value is different from the current object length, OSS returns the 409 message and the error code PositionNotEqualToLength. If such an error occurs, you can obtain the position for the next Append Object request from x-oss-next-append-position in the response header, and send an Append Object request again.
-   If the position value is 0 and an appendable object with the same name does not exist, or if the length of an appendable object with the same name is 0, the Append Object operation is successful; otherwise, the system regards that the position and object length are mismatched.
-   If the position value is 0 and an object with the same name does not exist, headers \(such as x-oss-server-side-encryption\) can be set in the Append Object request like the Put Object request. This is the same as the case of Initiate Multipart Upload. If the position value is 0, and the correct x-oss-server-side-encryption header is added to the request, the header of the response to the subsequent Append Object request also contains x-oss-server-side-encryption, which indicates the encryption algorithm. Later, if meta must be modified, you can use the Copy Object request.
-   Because of the concurrency, even if you set the value of position to x-oss-next-append-position, this request can still fails owing to the PositionNotEqualToLength.
-   he length limit of an object generated by Append Object is the same as that of an object generated by Put Object.
-   After each Append Object operation, the last modification time of this object is updated.
-   If the position value is correct and the content with the length of 0 is appended to an existing appendable object, this operation does not change the status of the object.

## CRC64 computing method {#section_xb5_xpw_bz .section}

The CRC value of an appendable object is computed according to [ECMA-182 Standard](http://www.ecma-international.org/publications/standards/Ecma-182.htm). Its computing method is the same as that of XZ. CRC64 can be computed as follows using the boost CRC module:

```
typedef boost::crc_optimal<64, 0x42F0E1EBA9EA3693ULL, 0xffffffffffffffffULL, 0xffffffffffffffffULL, true, true> boost_ecma;

uint64_t do_boost_crc(const char* buffer, int length)

    boost_ecma crc;
    crc.process_bytes(buffer, length);
    return crc.checksum();

```

Alternatively, CRC64 can be computed as follows using the Python crcmod:

```
do_crc64 = crcmod.mkCrcFun(0x142F0E1EBA9EA3693L, initCrc=0L, xorOut=0xffffffffffffffffL, rev=True)

print do_crc64(“123456789”)
```

## Example {#section_phh_cqw_bz .section}

**Request example:**

```
POST /oss.jpg? append&position=0 HTTP/1.1
Host: oss-example.oss.aliyuncs.com
Cache-control: no-cache
Expires: Wed, 08 Jul 2015 16:57:01 GMT
Content-Encoding: utf-8
Content-Disposition: attachment;filename=oss_download.jpg
Date: Wed, 08 Jul 2015 06:57:01 GMT
Content-Type: image/jpg
Content-Length: 1717
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk=

[1717 bytes of object data]

```

**Return example:**

```
HTTP/1.1 200 OK
Date: Wed, 08 Jul 2015 06:57:01 GMT
ETag: "0F7230CAA4BE94CCBDC99C5500000000"
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
x-oss-hash-crc64ecma: 14741617095266562575
x-oss-next-append-position: 1717
x-oss-request-id: 559CC9BDC755F95A64485981
```

