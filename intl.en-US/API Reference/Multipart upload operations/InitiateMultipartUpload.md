# InitiateMultipartUpload {#reference_zgh_cnx_wdb .reference}

Before transmitting data in the Multipart Upload mode, you must call the Initiate Multipart Upload interface to notify OSS to initiate a Multipart Upload event.

The Initiate Multipart Upload interface returns a globally unique Upload ID created by the OSS server to identify this Multipart Upload event. You can initiate operations based on this ID, such as aborting Multipart Upload and querying Multipart Upload.

## Request syntax {#section_zpz_gnx_wdb .section}

```
POST /ObjectName? uploads HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT date
Authorization: SignatureValue
```

## Request parameters {#section_n32_jnx_wdb .section}

During the Initiate Multipart Upload operation, you can use encoding-type to encode the Key in the returned result.

|Name|Type|Description|
|:---|:---|:----------|
|encoding-type|String|Specify the encoding type of the Key in the returned result. Currently, the URL encoding is supported. The Key adopts UTF-8 encoding, but the XML 1.0 Standard does not support parsing certain control characters, such as the characters with ASCII values from 0 to 10. In case, if the Key contains control characters not supported by the XML 1.0 Standard, you can specify the encoding-type to encode the returned Key. Default: None

Optional value: url

|

## Request header {#section_mny_pnx_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|Cache-Control|String|Specify the web page caching behavior when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default: None

|
|Content-Disposition|String|Specify the name of the object when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt).Default: None

|
|Content-Encoding|字符串|Specify the content encoding format when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default: None

|
|Expires|Integer|Specify the expiration time in milliseconds. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default: None

|
|x-oss-server-side-encryption|String|Specify the server-side encryption algorithm used to upload each part of this object. OSS stores each uploaded part based on server-side encryption. Legal value: AES256 or KMS 

Attention: You must enable the KMS \(Key Management Service\) on the console to use the KMS encryption algorithm. Otherwise, a KmsServiceNotenabled error code is reported.

|

## Response elements {#section_y4f_b4x_wdb .section}

|Name|Type|Description|
|:---|:---|:----------|
|Bucket|String|Name of a bucket for which a Multipart Upload event is initiated. Parent node: InitiateMultipartUploadResult

|
|InitiateMultipartUploadResult|Container|The container that saves the result of the Initiate Multipart Upload request.Child Nodes: Bucket, Key, UploadId

Parent node: None

|
|Key|String|Name of an object for which a Multipart Upload event is initiated.Parent node: InitiateMultipartUploadResult

|
|UploadId|String|Unique ID of a Multipart Upload event.Parent node: InitiateMultipartUploadResult

|
|EncodingType|String|Specify the encoding type for the returned results. If encoding-type is specified in the request, the Key is encoded in the returned result.Parent node: Container

|

## Detail analysis {#section_dvp_l4x_wdb .section}

-   When using this operation to calculate the authentication signature, you must add “?uploads” to “CanonicalizedResource”.
-   The Initiate Multipart Upload request supports the following standard HTTP request headers:  Cache-Control, Content-Disposition, Content-Encoding, Content-Type, Expires, and custom headers starting with x-oss-meta-. For the specific meaning of these headers, see the PUT Object interface.
-   The Initiate Multipart Upload request does not affect an existing object with the same name.
-   When receiving an Initiate Multipart Upload request, the server returns a request body in XML format. The request body has three elements: Bucket, Key, and UploadID. You must record the UploadID for subsequent Multipart operations.
-   If the x-oss-server-side-encryption header is set in the Initiate Multipart Upload request, the server returns this header in the response header. During the upload of each part, the server automatically stores the part based on entropy encryption. Currently, the OSS server only supports the 256-bit advanced encryption standard \(AES256\). If values of other standards are specified, the OSS server returns the error code 400 and the error message InvalidEncryptionAlgorithmError. When uploading each part, you do not need to add the x-oss-server-side-encryption request header. If this request header is specified, OSS returns the error code 400 and the error message InvalidArgument.

## Example {#section_u1v_p4x_wdb .section}

**Request example:**

```
POST /multipart.data? uploads HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 22 Feb 2012 08:32:21 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:/cluRFtRwMTZpC2hTj4F67AGdM4=
```

**Response example:**

```
HTTP/1.1 200 OK
Content-Length: 230
Server: AliyunOSS
Connection: keep-alive
x-oss-request-id: 42c25703-7503-fbd8-670a-bda01eaec618
Date: Wed, 22 Feb 2012 08:32:21 GMT
Content-Type: application/xml
<? xml version="1.0" encoding="UTF-8"? >
<InitiateMultipartUploadResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Bucket> multipart_upload</Bucket>
    <Key>multipart.data</Key>
    <UploadId>0004B9894A22E5B1888A1E29F8236E2D</UploadId>
</InitiateMultipartUploadResult>
```

