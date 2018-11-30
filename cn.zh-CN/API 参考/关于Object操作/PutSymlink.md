# PutSymlink {#reference_qzz_qzw_wdb .reference}

PutSymlink可用于针对OSS上的TargetObject创建软链接，用户可以通过该软链接访问TargetObject。

## 请求语法 {#section_ndm_5zw_wdb .section}

```
PUT /ObjectName?symlink HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-symlink-target: TargetObjectName
```

## 请求Header {#section_ztg_wzw_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|x-oss-symlink-target|字符串|软链接指向的目标文件。合法值：命名规范同Object。

|
|x-oss-storage-class|字符串|指定Object的存储类型。取值：

-   Standard
-   IA
-   Archive

支持的接口：PutObject、InitMultipartUpload、AppendObject、 PutObjectSymlink、CopyObject。

**说明：** 

-   如果StorageClass的值不合法，返回400 错误。错误码：InvalidArgument。
-   PutObjectSymlink的存储类型建议不要指定为IA或Archive类型（因为IA与Archive类型的单个文件如不足64KB，会按64KB计量计费）。
-   对于任意存储类型Bucket，若上传Object时指定该值，则此次上传的Object将存储为指定的类型。例如，在IA类型的Bucket中上传Object时，若指定x-oss-storage-class为Standard，则该Object直接存储为Standard。

|

## 细节分析 {#section_zn5_d1x_wdb .section}

-   TargetObjectName同ObjectName一样，需要URL encode。
-   软链接的目标文件类型不能为软链接。
-   创建软链接：

    -   不检查目标文件是否存在
    -   不检查目标文件类型是否合法
    -   不检查目标文件是否有权限访问
    以上检查，都推迟到GetObject等需要访问目标文件的API。

-   试图添加的文件已经存在，并且有访问权限。新添加的文件将覆盖原来的文件，成功将返回200 OK。
-   PutSymlink时，携带以x-oss-meta-为前缀的参数，则视为user meta，比如x-oss-meta-location。一个Object可以有多个类似的参数，但所有的user meta总大小不能超过8KB。

## 示例 {#section_th3_m1x_wdb .section}

请求示例：

```
PUT /link-to-oss.jpg?symlink HTTP/1.1 
Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
Cache-control: no-cache 
Content-Disposition: attachment;filename=oss_download.jpg 
Date: Tue, 08 Nov 2016 02:00:25 GMT 
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk= x-oss-symlink-target: oss.jpg
x-oss-storage-class: Standard
```

返回示例：

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Tue, 08 Nov 2016 02:00:25 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 582131B9109F4EE66CDE56A5
ETag: "0A477B89B4602AA8DECB8E19BFD447B6"
```

