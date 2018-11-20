# InitiateMultipartUpload {#reference_zgh_cnx_wdb .reference}

使用Multipart Upload模式传输数据前，必须先调用该接口来通知OSS初始化一个Multipart Upload事件。

该接口会返回一个OSS服务器创建的全局唯一的Upload ID，用于标识本次Multipart Upload事件。用户可以根据这个ID来发起相关的操作，如中止Multipart Upload、查询Multipart Upload等。

## 请求语法 {#section_zpz_gnx_wdb .section}

```
POST /ObjectName?uploads HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT date
Authorization: SignatureValue
```

## 请求参数\(Request Parameters\) {#section_n32_jnx_wdb .section}

Initiate Multipart Upload时，可以通过encoding-type对返回结果中的Key进行编码。

|名称|类型|描述|
|:-|:-|:-|
|encoding-type|字符串|指定对返回的Key进行编码，目前支持url编码。Key使用UTF-8字符，但xml 1.0标准不支持解析一些控制字符，比如ascii值从0到10的字符。对于Key中包含xml 1.0标准不支持的控制字符，可以通过指定encoding-type对返回的Key进行编码。默认值：无

可选值：url

|

## 请求Header {#section_mny_pnx_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|Cache-Control|字符串|指定该Object被下载时的网页的缓存行为；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。默认值：无

|
|Content-Disposition|字符串|指定该Object被下载时的名称；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。默认值：无

|
|Content-Encoding|字符串|指定该Object被下载时的内容编码格式；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。默认值：无

|
|Expires|整数|过期时间（milliseconds）；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。默认值：无

|
|x-oss-server-side-encryption|字符串|指定上传该Object每个part时使用的服务器端加密编码算法，OSS会对上传的每个part采用服务器端加密编码进行存储。合法值：AES256 或 KMS 

注意：用户需要在控制台开通KMS（密钥管理服务），才可使用KMS加密算法，否则会报KmsServiceNotEnabled错误码。

|
|x-oss-storage-class|字符串|指定Object的存储类型。取值：

-   Standard
-   IA
-   Archive

支持的接口：PutObject、InitMultipartUpload、AppendObject、 PutObjectSymlink、CopyObject。

**说明：** 

-   如果StorageClass的值不合法，返回400 错误。错误码：InvalidArgumet。
-   对于任意存储类型Bucket，若上传Object时指定该值，则此次上传的Object将存储为指定的类型。例如，在IA类型的Bucket中上传Object时，若指定x-oss-storage-class为Standard，则该Object直接存储为Standard。

|

## 响应元素\(Response Elements\) {#section_y4f_b4x_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|Bucket|字符串|初始化一个Multipart Upload事件的Bucket名称。父节点：InitiateMultipartUploadResult

|
|InitiateMultipartUploadResult|容器|保存Initiate Multipart Upload请求结果的容器。子节点：Bucket, Key, UploadId

父节点：None

|
|Key|字符串|初始化一个Multipart Upload事件的Object名称。父节点：InitiateMultipartUploadResult

|
|UploadId|字符串|唯一标示此次Multipart Upload事件的ID。父节点：InitiateMultipartUploadResult

|
|EncodingType|字符串|指明返回结果中编码使用的类型。如果请求的参数中指定了encoding-type，那返回的结果会对Key进行编码。父节点：容器

|

## 细节分析 {#section_dvp_l4x_wdb .section}

-   该操作计算认证签名的时候，需要加“?uploads”到CanonicalizedResource中。
-   初始化Multipart Upload请求，支持如下标准的HTTP请求头：Cache-Control、Content-Disposition、Content-Encoding、Content-Type、Expires，以及以`x-oss-meta-`开头的用户自定义Headers。具体含义请参见[PutObject](intl.zh-CN/API 参考/关于Object操作/PutObject.md#)。
-   初始化Multipart Upload请求，并不会影响已经存在的同名Object。
-   服务器收到初始化Multipart Upload请求后，会返回一个XML格式的请求体。该请求体内有三个元素：Bucket、Key和UploadID。请记录下其中的UploadID，以用于后续的Multipart相关操作。
-   初始化Multipart Upload请求时，若设置了x-oss-server-side-encryption Header，则在响应头中会返回该Header，并且在上传的每个part时，服务端会自动对每个part进行熵编码加密存储，目前OSS服务器端只支持AES256和KMS加密，指定其他值会返回400 错误和错误码：InvalidEncryptionAlgorithmError；在上传每个part时不必再添加x-oss-server-side-encryption 请求头，若指定该请求头则OSS会返回400 错误和错误码：InvalidArgument。

## 示例 {#section_u1v_p4x_wdb .section}

请求示例：

```
POST /multipart.data?uploads HTTP/1.1 
Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
Date: Wed, 22 Feb 2012 08:32:21 GMT 
x-oss-storage-class: Archive
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:/cluRFtRwMTZpC2hTj4F67AGdM4=
```

返回示例：

```
HTTP/1.1 200 OK
Content-Length: 230
Server: AliyunOSS
Connection: keep-alive
x-oss-request-id: 42c25703-7503-fbd8-670a-bda01eaec618
Date: Wed, 22 Feb 2012 08:32:21 GMT
Content-Type: application/xml
<?xml version="1.0" encoding="UTF-8"?>
<InitiateMultipartUploadResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Bucket> multipart_upload</Bucket>
    <Key>multipart.data</Key>
    <UploadId>0004B9894A22E5B1888A1E29F8236E2D</UploadId>
</InitiateMultipartUploadResult>
```

