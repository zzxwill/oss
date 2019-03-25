# UploadPart {#reference_pnq_2px_wdb .reference}

初始化一个MultipartUpload之后，可以根据指定的Object名和Upload ID来分块（Part）上传数据。每一个上传的Part都有一个标识它的号码（part number，范围是1-10000）。

对于同一个Upload ID，该号码不但唯一标识这一块数据，也标识了这块数据在整个文件内的相对位置。如果你用同一个part号码，上传了新的数据，那么OSS上已有的这个号码的Part数据将被覆盖。分片数量范围1-10000，单个分片大小范围100KB-5GB，最后一片可以小于100KB。

## 请求语法 {#section_efc_3px_wdb .section}

```
PUT /ObjectName?partNumber=PartNumber&uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: Size
Authorization: SignatureValue
```

## 细节分析 {#section_osz_jpx_wdb .section}

-   调用该接口上传Part数据前，必须调用InitiateMultipartUpload接口，获取一个OSS服务器颁发的Upload ID。
-   MultipartUpload要求除最后一个Part以外，其他的Part大小都要大于100KB。但是UploadPart接口并不会立即校验上传Part的大小（因为不知道是否为最后一块）；只有当CompleteMultipartUpload的时候才会校验。
-   OSS会将服务器端收到Part数据的MD5值放在ETag头内返回给用户。
-   Part号码的范围是1-10000。如果超出这个范围，OSS将返回InvalidArgument的错误码。
-   若调用InitiateMultipartUpload接口时，指定了x-oss-server-side-encryption请求头，则会对上传的Part进行加密编码，并在UploadPart响应头中返回x-oss-server-side-encryption头，其值表明该Part的服务器端加密算法，具体见[InitiateMultipartUpload接口](intl.zh-CN/API 参考/关于MultipartUpload的操作/InitiateMultipartUpload.md)。
-   为了保证数据在网络传输过程中不出现错误，用户发送请求时需要携带Content-MD5，OSS会计算上传数据的MD5并与用户上传的MD5值比较，如果不一致返回InvalidDigest错误码。

## 示例 {#section_xbd_4px_wdb .section}

**请求示例**

```
PUT /multipart.data?partNumber=1&uploadId=0004B9895DBBB6EC98E36  HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length：6291456
Date: Wed, 22 Feb 2012 08:32:21 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:J/lICfXEvPmmSW86bBAfMmUmWjI=
[6291456 bytes data]
```

**返回示例**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Connection: keep-alive
ETag: 7265F4D211B56873A381D321F586E4A9
x-oss-request-id: 3e6aba62-1eae-d246-6118-8ff42cd0c21a
Date: Wed, 22 Feb 2012 08:32:21 GMT
```

