# CopyObject {#reference_mvx_xxc_5db .reference}

CopyObject接口用于在同一个地域内将一个Object（源Object）拷贝到另外一个Object（目标Object）。

该接口可以发送一个Put请求给OSS，并在Put请求Header中添加元素`x-oss-copy-source`来指定源Object。OSS会自动判断出这是一个拷贝操作，并直接在服务器端执行该操作。如果拷贝成功，则返回目标Object信息给用户。

## 请求语法 {#section_jzf_fmr_xdb .section}

```
PUT /DestObjectName HTTP/1.1
Host: DestBucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-copy-source: /SourceBucketName/SourceObjectName
```

## 请求Header {#section_vdd_klw_bz .section}

|名称|类型|描述|
|--|--|--|
|x-oss-copy-source|字符串|指定拷贝的源地址。 默认值：无

|
|x-oss-copy-source-if-match|字符串|如果源Object的ETag值和用户提供的ETag相等，则执行拷贝操作，并返回200；否则返回412 HTTP错误码（预处理失败）。 默认值：无

|
|x-oss-copy-source-if-none-match|字符串|如果源Object的ETag值和用户提供的ETag不相等，则执行拷贝操作，并返回200；否则返回304 HTTP错误码（预处理失败）。 默认值：无

|
|x-oss-copy-source-if-unmodified-since|字符串|如果传入参数中的时间等于或者晚于文件实际修改时间，则正常传输文件，并返回200 OK；否则返回412 precondition failed错误。 默认值：无

|
|x-oss-copy-source-if-modified-since|字符串|如果源Object自从用户指定的时间以后被修改过，则执行拷贝操作；否则返回304 HTTP错误码（预处理失败）。 默认值：无

|
|x-oss-metadata-directive|字符串| 指定如何设置目标Object的元信息。有效值为COPY和REPLACE。

-   COPY （默认值）：复制源Object的元信息到目标Object。注意源Object的x-oss-server-side-encryption不会被复制，即目标Object是否进行服务器端加密编码只根据COPY操作是否指定了x-oss-server-side-encryption请求Header来决定。
-   REPLACE：忽略源Object的元信息，直接采用用户请求中指定的元信息。

**说明：** 如果拷贝操作的源Object地址和目标Object地址相同，则无论x-oss-metadata-directive为何值，都会直接替换源Object的元信息。

 |
|x-oss-server-side-encryption|字符串|指定OSS创建目标Object时的服务器端熵编码加密算法 。取值：

-   AES256
-   KMS（您需要购买KMS套件，才可以使用KMS加密算法，否则会报KmsServiceNotEnabled错误码）

**说明：** 

-   如果指定了x-oss-server-side-encryption，则无论源Object是否进行过服务器端加密编码，拷贝之后的目标Object都会进行服务器端加密编码。并且拷贝操作的响应头中会包含x-oss-server-side-encryption，值为目标Object的加密算法。在目标Object被下载时，响应头中也会包含x-oss-server-side-encryption，值为该Object的加密算法。
-   若拷贝操作中未指定x-oss-server-side-encryption，则无论源Object是否进行过服务器端加密编码，拷贝之后的目标Object都是未进行过服务器端加密编码的数据。

|
|x-oss-object-acl|字符串|指定OSS创建目标Object时的访问权限。 取值：

-   public-read
-   private
-   public-read-write

|

## 响应元素\(Response Elements\) {#section_tvz_qlw_bz .section}

|名称|类型|描述|
|--|--|--|
|CopyObjectResult|字符串|CopyObject的结果。默认值：无

|
|ETag|字符串|目标Object的ETag值。父元素：CopyObjectResult

|
|LastModified|字符串|目标Object最后更新时间。父元素：CopyObjectResult

|

## 细节分析 {#section_cqk_tlw_bz .section}

-   限制条件
    -   仅支持小于1GB的文件。如果要拷贝大于1GB的文件，必须使用MultipartUpload操作，详情请参见[UploadPartCopy](intl.zh-CN/API 参考/关于MultipartUpload的操作/UploadPartCopy.md#)。
    -   请求者必须对源Object有读权限。
    -   源Object和目标Object必须属于同一个地域（数据中心）。
    -   不能拷贝通过追加上传方式产生的Object。
    -   如果源Object为符号链接，只拷贝符号链接，不拷贝符号链接指向的文件内容。
-   计量计费
    -   对源Object所在的Bucket增加一次Get请求次数。
    -   对目标Object所在的Bucket增加一次Put请求次数。
    -   对目标Object所在的Bucket增加相应的存储量。
-   预判断请求Header
    -   四个预判断请求Header（x-oss-copy-source-if-match、x-oss-copy-source-if-none-match、x-oss-copy-source-if-unmodified-since、x-oss-copy-source-if-modified-since）中，任意个数可同时出现。相应逻辑请参见GetObject操作中的[细节分析](intl.zh-CN/API 参考/关于Object操作/GetObject.md#section_xb4_wmw_bz)。
    -   拷贝操作涉及到的请求Header都是以x-oss-开头的，所以要加到签名字符串中。

## 示例 {#section_osk_5lw_bz .section}

**请求示例：**

```
PUT /copy_oss.jpg HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 07:18:48 GMT
x-oss-copy-source: /oss-example/oss.jpg
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:gmnwPKuu20LQEjd+iPkL259A+n0=
```

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Content-Type: application/xml
Content-Length: 193
Connection: keep-alive
Date: Fri, 24 Feb 2012 07:18:48 GMT
Server: AliyunOSS
<?xml version="1.0" encoding="UTF-8"?>
<CopyObjectResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
 <LastModified>Fri, 24 Feb 2012 07:18:48 GMT</LastModified>
 <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE"</ETag>
</CopyObjectResult>
```

