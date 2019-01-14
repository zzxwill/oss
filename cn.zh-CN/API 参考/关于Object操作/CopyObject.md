# CopyObject {#reference_mvx_xxc_5db .reference}

CopyObject操作用于在 Bucket 内或同地域的 Bucket 之间拷贝文件。

通过CopyObject接口可以发送一个 Put 请求给 OSS，并在 Put 请求 Header 中添加元素`x-oss-copy-source`来指定源 Object。OSS 会自动判断出这是一个拷贝操作，并直接在服务器端执行该操作。如果拷贝成功，则返回目标 Object 信息给用户。

## 请求语法 {#section_jzf_fmr_xdb .section}

```
PUT /DestObjectName HTTP/1.1
Host: DestBucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-copy-source: /SourceBucketName/SourceObjectName
```

## 请求 Header {#section_vdd_klw_bz .section}

|名称|类型|描述|
|--|--|--|
|x-oss-copy-source|字符串|指定拷贝的源地址。 默认值：无

|
|x-oss-copy-source-if-match|字符串|如果源 Object 的 ETag 值和用户提供的ETag 相等，则执行拷贝操作，并返回 200 OK；否则返回 412 HTTP 错误码（预处理失败）。 默认值：无

|
|x-oss-copy-source-if-none-match|字符串|如果源Object的ETag值和用户提供的ETag不相等，则执行拷贝操作，并返回 200 OK；否则返回 304 HTTP 错误码（预处理失败）。 默认值：无

|
|x-oss-copy-source-if-unmodified-since|字符串|如果传入参数中的时间等于或者晚于文件实际修改时间，则正常拷贝文件，并返回 200 OK；否则返回 412 precondition failed 错误。 默认值：无

|
|x-oss-copy-source-if-modified-since|字符串|如果源 Object 自从用户指定的时间以后被修改过，则执行拷贝操作；否则返回 304 HTTP 错误码（预处理失败）。 默认值：无

|
|x-oss-metadata-directive|字符串| 指定如何设置目标 Object 的元信息。有效值为 COPY 和 REPLACE 。

-   COPY （默认值）：复制源 Object 的元信息到目标 Object。注意，源 Object 的x-oss-server-side-encryption不会被复制，即目标Object是否进行服务器端加密编码只根据 COPY 操作是否指定了x-oss-server-side-encryption请求Header来决定。
-   REPLACE：忽略源 Object 的元信息，直接采用用户请求中指定的元信息。

**说明：** 如果拷贝操作的源 Object 地址和目标Object 地址相同，则无论x-oss-metadata-directive为何值，都会直接替换源 Object 的元信息。

 |
|x-oss-server-side-encryption|字符串|指定 OSS 创建目标 Object 时的服务器端熵编码加密算法 。取值：

-   AES256
-   KMS（您需要购买KMS套件，才可以使用 KMS 加密算法，否则会报 KmsServiceNotEnabled 错误码）

**说明：** 

-   如果拷贝操作中未指定x-oss-server-side-encryption，则无论源 Object 是否进行过服务器端加密编码，拷贝之后的目标 Object 都未进行服务器端加密编码。
-   如果拷贝操作中指定了x-oss-server-side-encryption，则无论源 Object 是否进行过服务器端加密编码，拷贝之后的目标 Object 都会进行服务器端加密编码。并且拷贝操作的响应头中会包含x-oss-server-side-encryption，值为目标 Object 的加密算法。在目标Object 被下载时，响应头中也会包含x-oss-server-side-encryption，值为该Object的加密算法。

|
|x-oss-server-side-encryption-key-id|字符串|表示KMS托管的用户主密钥。该参数在x-oss-server-side-encryption为KMS时有效。

|
|x-oss-object-acl|字符串|指定OSS创建目标Object时的访问权限。 取值：

-   public-read
-   private
-   public-read-write
-   default

|
|x-oss-storage-class|字符串|指定Object的存储类型。取值：

-   Standard
-   IA
-   Archive

支持的接口：PutObject、InitMultipartUpload、AppendObject、 PutObjectSymlink、CopyObject。

**说明：** 

-   如果 StorageClass 的值不合法，返回 400 错误。错误码：InvalidArgument。
-   PutObjectSymlink 的存储类型建议不要指定为 IA或 Archive 类型（因为 IA 与 Archive 类型的单个文件如不足 64KB，会按 64KB计量计费）。
-   对于任意存储类型 Bucket，若上传 Object 时指定该值，则此次上传的 Object 将存储为指定的类型。例如，在 IA 类型的 Bucket 中上传 Object 时，若指定 x-oss-storage-class 为 Standard，则该 Object 直接存储为Standard。
-   更改 Object 存储类型涉及到数据覆盖，如果 IA 或Archive 类型 Object 分别在创建后 30 和 60 天内被覆盖，则它们会产生提前删除费用。比如，IA 类型Object 创建 10 天后，被覆盖成 Archive 或 Standard，则会产生 20 天的提前删除费用。

|

## 响应元素\(Response Elements\) {#section_tvz_qlw_bz .section}

|名称|类型|描述|
|--|--|--|
|CopyObjectResult|字符串|CopyObject 的结果。默认值：无

|
|ETag|字符串|目标 Object 的 ETag 值。父元素：CopyObjectResult

|
|LastModified|字符串|目标 Object 最后更新时间。父元素：CopyObjectResult

|

## 细节分析 {#section_cqk_tlw_bz .section}

-   限制条件
    -   仅支持小于 1GB 的文件。如果要拷贝大于 1GB 的文件，必须使用 MultipartUpload 操作，详情请参见[UploadPartCopy](cn.zh-CN/API 参考/关于MultipartUpload的操作/UploadPartCopy.md#)。
    -   请求者必须对源 Object 有读权限。
    -   源 Object 和目标 Object 必须属于同一地域（数据中心）。
    -   不能拷贝通过追加上传方式产生的 Object。
    -   如果源 Object 为软链接，只拷贝软链接，不拷贝软链接指向的文件内容。
-   计量计费
    -   对源 Object 所在的 Bucket 增加一次 Get 请求次数。
    -   对目标 Object 所在的 Bucket 增加一次 Put 请求次数。
    -   对目标 Object 所在的 Bucket 增加相应的存储量。
-   预判断请求Header
    -   四个预判断请求H eader（x-oss-copy-source-if-match、x-oss-copy-source-if-none-match、x-oss-copy-source-if-unmodified-since、x-oss-copy-source-if-modified-since）中，任意个数可同时出现。相应逻辑请参见 GetObject 操作中的[细节分析](cn.zh-CN/API 参考/关于Object操作/GetObject.md#section_xb4_wmw_bz)。
    -   拷贝操作涉及到的请求 Header 都是以x-oss-开头，所以要加到签名字符串中。

## 示例 {#section_osk_5lw_bz .section}

-   示例 1

    请求示例 ：

    ```
    PUT /copy_oss.jpg HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Fri, 24 Feb 2012 07:18:48 GMT 
    x-oss-storage-class: Archive
    x-oss-copy-source: /oss-example/oss.jpg 
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:gmnwPKuu20LQEjd+iPkL259A+n0=
    ```

    返回示例：

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

-   示例 2

    请求示例：

    ```
    PUT /test%2FAK.txt HTTP/1.1
    Host: tesx.oss-cn-zhangjiakou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: */*
    Connection: keep-alive
    x-oss-copy-source: /test/AK.txt
    date: Fri, 28 Dec 2018 09:41:55 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:gmnwPKuu20LQEjd+iPkL259A+n0=
    Content-Length: 0
    ```

    返回示例：

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Fri, 28 Dec 2018 09:41:56 GMT
    Content-Type: application/xml
    Content-Length: 184
    Connection: keep-alive
    x-oss-request-id: 5C25EFE4462CE00EC6D87156
    ETag: "F2064A169EE92E9775EE5324D0B1682E"
    x-oss-hash-crc64ecma: 12753002859196105360
    x-oss-server-time: 150
    
    <?xml version="1.0" encoding="UTF-8"?>
    <CopyObjectResult>
      <ETag>"F2064A169EE92E9775EE5324D0B1682E"</ETag>
      <LastModified>2018-12-28T09:41:56.000Z</LastModified>
    </CopyObjectResult>
    ```

    **说明：** x-oss-hash-crc64ecma 表明 Object 的 64 位 CRC值。该 64 位 CRC 根据[ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm)标准计算得出。进行 COPY 操作时，生成的 Object 不保证具有 crc64 的值。


