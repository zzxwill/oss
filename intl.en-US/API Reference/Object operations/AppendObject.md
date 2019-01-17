# AppendObject {#reference_fvf_xld_5db .reference}

AppendObject is used to upload a file by appending the file to an existing object.

An object created with the AppendObject operation is an appendable object, and an object uploaded with the PutObject operation is a normal object.

**Note:** You cannot use KMS to encrypt appendable objects on the server by specifying CMK IDs for them.

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
|Cache-Control|String|Specifies the Web page caching behavior when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: None

|
|Content-Disposition|String|Specifies the name of the object when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: None

|
|Content-Encoding|String|Specifies the content encoding format when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: None

|
|Content-MD5|String|As defined in RFC 1864, an MD5 value, which is a 128-bit number, is calculated based on the message content \(excluding the header\). This number is then Base64-encoded into a Content-MD5 value. This request header can be used to check the validity of a message, that is, whether the message content is consistent with the sent content. Although this request header is optional, we recommend that you use this request header for end-to-end checks. Default value: None

Restriction: None

|
|Expires|Integer|Specifies the expiration time. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: None

|
|x-oss-server-side-encryption|String|Specifies the server-side encryption algorithm when OSS creates an object. Valid value: AES256 or KMS

**Note:** You must enable KMS \(Key Management Service\) in the console before you can use the KMS encryption algorithm. Otherwise, a KmsServiceNotEnabled error code is returned.

|
|x-oss-object-acl|String|Specifies the access permission when OSS creates an object. Valid values: public-read, private, and public-read-write

|
|x-oss-storage-class|String|Specifies the storage class of the object.Values:

-   Standard
-   IA
-   Archive

Supported interfaces: PutObject, InitMultipartUpload, AppendObject, PutObjectSymlink, and CopyObject

**Note:** 

-   If the value of StorageClass is invalid, a 400 error is returned. Error code: InvalidArgument
-   If you specify the value of x-oss-storage-class when uploading an object to a bucket, the storage class of the uploaded object is the specified value of x-oss-storage-class regardless of the storage class of the bucket. For example, if you specify the value of x-oss-storage-class to Standard when uploading an object to a bucket of the IA storage class, the storage class of the object is Standard.
-   This header takes effect only if you specify it when you perform the AppendObject operation for the first time.

|

## Response header {#section_xbv_rpw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|x-oss-next-append-position|64-bit integer|Specifies the position that must be provided in the next request. It indicates the current object length. This header is carried when a successful message is returned for an AppendObject request, or when a 409 error occurs because the position and the object length do not match.|
|x-oss-hash-crc64ecma|64-bit integer|Specifies the 64-bit CRC value of the object. This value is calculated according to the [ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm).|

## Association with other operations {#section_vgd_wpw_bz .section}

-   The AppendObject operation is not applicable to a non-appendable object. For example, if the AppendObject operation is performed when a normal object with the same name already exists, a 409 message is returned with the ObjectNotAppendable error code.
-   If you perform the PutObject operation on an existing appendable object, this appendable object is overwritten by the new object, and the type of this object is changed to normal.
-   After the HeadObject operation is performed, the system returns x-oss-object-type that indicates the type of the object. If the object is an appendable object, the value of x-oss-object-type is Appendable. For an appendable object, after the HeadObject operation is performed, the system returns x-oss-next-append-position and x-oss-hash-crc64ecma.
-   In the response XML for a GetBucket \(ListObjects\) request, the Type of the appendable object is set to Appendable.
-   You can neither use CopyObject to copy an appendable object, nor change the server-side encryption of this object. However, you can use CopyObject to modify the custom metadata of an object.

## Detail analysis {#section_jxx_wpw_bz .section}

-   The two URL parameters, append and position, are both CanonicalizedResource, and must be included in the signature.
    -   URL parameters must also include append, which specifies that the operation is AppendObject.
    -   URL query parameters must include position, which specifies the position from where the append of the object starts. The value of position in the first AppendObject operation must be 0, and the value of position in the subsequent operation is the current object length. For example, if the value of position specified in the first AppendObject request is 0, and the value of content-length is 65536, the value of position specified in the second AppendObject request must be set to 65536. Each time after an operation succeeds, x-oss-next-append-position in the response header specifies the position of the next AppendObject request.
-   If the position value is 0
    -   and an appendable object with the same name does not exist, or if the length of an appendable object with the same name is 0, the AppendObject operation is successful. Otherwise, the system determines that the position and object length do not match.
    -   If the position value is 0 and an object with the same name does not exist, headers \(such as x-oss-server-side-encryption\) set in the PutObject request can be set in the AppendObject request. This action is the same as that of InitiateMultipartUpload. If the position value is 0, and the correct x-oss-server-side-encryption header is added to the request, the header of the response to the subsequent AppendObject request also contains the x-oss-server-side-encryption header that indicates the encryption algorithm. Afterwards, if the meta must be modified, you can use the CopyObject request.
-   If the position value is different from the current object length, the OSS returns a 409 error with the PositionNotEqualToLength error code. If such an error occurs, you can obtain the position for the next AppendObject request from x-oss-next-append-position in the response header, and send an AppendObject request again.
-   However, because of concurrency, even if you set the value of position to x-oss-next-append-position, this request may still fail due to the PositionNotEqualToLength error.
-   The length limit of an object generated by AppendObject is the same as that of an object generated by PutObject. Each time after an AppendObject operation is performed, the last modification time of this object is updated.
-   If the position value is correct and the content with a length of 0 is appended to an existing appendable object, the status of the object does not change.

## CRC64 calculation method {#section_xb5_xpw_bz .section}

The CRC value of an appendable object is calculated according to the [ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm). The calculation method is the same as that of XZ. CRC64 can be calculated using the boost CRC module as follows:

```
typedef boost::crc_optimal<64, 0x42F0E1EBA9EA3693ULL, 0xffffffffffffffffULL, 0xffffffffffffffffULL, true, true> boost_ecma;

uint64_t do_boost_crc(const char* buffer, int length)
{
    boost_ecma crc;
    crc.process_bytes(buffer, length);
    return crc.checksum();
}
```

Alternatively, CRC64 can be calculated using the Python crcmod as follows:

```
do_crc64 = crcmod.mkCrcFun(0x142F0E1EBA9EA3693L, initCrc=0L, xorOut=0xffffffffffffffffL, rev=True)

print do_crc64(“123456789”)
```

## Example {#section_phh_cqw_bz .section}

Request example:

```
POST /oss.jpg? append&position=0 HTTP/1.1 
Host: oss-example.oss.aliyuncs.com 
Cache-control: no-cache 
Expires: Wed, 08 Jul 2015 16:57:01 GMT 
Content-Encoding: utf-8 
x-oss-storage-class: Archive
Content-Disposition: attachment;filename=oss_download.jpg 
Date: Wed, 08 Jul 2015 06:57:01 GMT 
Content-Type: image/jpg 
Content-Length: 1717 
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk=  
[1717 bytes of object data]
```

Response example:

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

