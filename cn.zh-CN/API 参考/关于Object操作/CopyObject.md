# CopyObject {#reference_mvx_xxc_5db .reference}

CopyObject接口用于拷贝一个在OSS上已经存在的object成另外一个object。

该接口可以发送一个PUT请求给OSS，并在PUT请求头中添加元素`x-oss-copy-source`来指定拷贝源。OSS会自动判断出这是一个Copy操作，并直接在服务器端执行该操作。如果拷贝成功，则返回新的object信息给用户。

该操作适用于拷贝小于1GB的文件，当拷贝一个大于1GB的文件时，必须使用Multipart Upload操作，具体见Upload Part Copy。

Copy操作的源Bucket和目标Bucket必须是同一个Region，不同Region的Bucket不能执行Copy Object操作。

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
|x-oss-copy-source|字符串|复制源地址（必须有可读权限） 默认值：无

|
|x-oss-copy-source-if-match|字符串|如果源Object的ETag值和用户提供的ETag相等，则执行拷贝操作，并返回200；否则返回412 HTTP错误码（预处理失败）。 默认值：无

|
|x-oss-copy-source-if-none-match|字符串|如果源Object的ETag值和用户提供的ETag不相等，则执行拷贝操作，并返回200；否则返回304 HTTP错误码（预处理失败）。 默认值：无

|
|x-oss-copy-source-if-unmodified-since|字符串|如果传入参数中的时间等于或者晚于文件实际修改时间，则正常传输文件，并返回200 OK；否则返回412 precondition failed错误。 默认值：无

|
|x-oss-copy-source-if-modified-since|字符串|如果源Object自从用户指定的时间以后被修改过，则执行拷贝操作；否则返回304 HTTP错误码（预处理失败）。 默认值：无

|
|x-oss-metadata-directive|字符串|有效值为COPY和REPLACE。如果该值设为COPY，则新的Object的meta都从源Object复制过来；如果设为REPLACE，则忽视所有源Object的meta值，而采用用户这次请求中指定的meta值；其他值则返回400 HTTP错误码。注意该值为COPY时，源Object的x-oss-server-side-encryption的meta值不会进行拷贝。取值：

-   COPY （默认值）
-   REPLACE

|
|x-oss-server-side-encryption|字符串|指定oss创建目标object时的服务器端熵编码加密算法 取值：AES256或 KMS

**说明：** 您需要购买KMS套件，才可以使用KMS加密算法，否则会报KmsServiceNotEnabled错误码。

|
|x-oss-object-acl|字符串|指定oss创建object时的访问权限。 取值：public-read，private，public-read-write

|

## 响应元素\(Response Elements\) {#section_tvz_qlw_bz .section}

|名称|类型|描述|
|--|--|--|
|CopyObjectResult|字符串|Copy Object的结果。默认值：无

|
|ETag|字符串|新Object的ETag值。父元素：CopyObjectResult

|
|LastModified|字符串|新Object最后更新时间。父元素：CopyObjectResult

|

## 细节分析 {#section_cqk_tlw_bz .section}

-   可以通过拷贝操作来实现修改已有Object的meta信息。
-   如果拷贝操作的源Object地址和目标Object地址相同，则无论x-oss-metadata-directive为何值，都会直接替换源Object的meta信息。
-   OSS支持拷贝操作的四个预判断Header任意个同时出现，相应逻辑参见Get Object操作的细节分析。
-   拷贝操作需要请求者对源Object有读权限。
-   源Object和目标Object必须属于同一个数据中心，否则返回403 AccessDenied，错误信息为：Target object does not reside in the same data center as source object。
-   拷贝操作的计费统计会对源Object所在的Bucket增加一次Get请求次数，并对目标Object所在的Bucket增加一次Put请求次数，以及相应的新增存储空间。
-   拷贝操作涉及到的请求头，都是以“x-oss-”开头的，所以要加入签名字符串中。
-   若在拷贝操作中指定了x-oss-server-side-encryption请求头，并且请求值合法（为AES256），则无论源Object是否进行过服务器端加密编码，拷贝之后的目标Object都会进行服务器端加密编码。并且拷贝操作的响应头中会包含x-oss-server-side-encryption，值被设置成目标Object的加密算法。在这个目标Object被下载时，响应头中也会包含x-oss-server-side-encryption，值被设置成该Object的加密算法；若拷贝操作中未指定x-oss-server-side-encryption请求头，则无论源Object是否进行过服务器端加密编码，拷贝之后的目标Object都是未进行过服务器端加密编码加密的数据。
-   拷贝操作中x-oss-metadata-directive请求头为COPY（默认值）时，并不拷贝源Object的x-oss-server-side-encryption值，即目标Object是否进行服务器端加密编码只根据COPY操作是否指定了x-oss-server-side-encryption请求头来决定。
-   若在拷贝操作中指定了x-oss-server-side-encryption请求头，并且请求值非AES256，则返回400和相应的错误提示：InvalidEncryptionAlgorithmError。
-   如果拷贝的文件大小大于1GB，会返回400和错误提示：EntityTooLarge。
-   该操作不能拷贝通过Append追加上传方式产生的object。
-   如果文件类型为符号链接，只拷贝符号链接。

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

