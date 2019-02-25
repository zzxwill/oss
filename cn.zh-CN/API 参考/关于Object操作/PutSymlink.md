# PutSymlink {#reference_qzz_qzw_wdb .reference}

PutSymlink接口用于为OSS的TargetObject创建软链接，您可以通过该软链接访问TargetObject。

## 请求语法 {#section_ndm_5zw_wdb .section}

```
PUT /ObjectName?symlink HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-symlink-target: TargetObjectName
```

## 请求头 {#section_ztg_wzw_wdb .section}

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|x-oss-symlink-target|字符串|是| 软链接指向的目标文件。

 合法值：命名规范同Object

 **说明：** 

-   TargetObjectName同ObjectName一样，需要URL encode。
-   软链接的目标文件类型不能为软链接。

 |
|x-oss-storage-class|字符串|否| 指定Object的存储类型。

 取值：Standard、IA、Archive

 **说明：** 

-   IA与Archive类型的单个文件如不足64 KB，会按64 KB计量计费。建议您在使用Symlink接口时不要将Object的存储类型指定为IA或Archive。
-   对于任意存储类型的Bucket，若上传Object时指定此参数，则此次上传的Object将存储为指定的类型。例如，在IA类型的Bucket中上传Object时，若指定x-oss-storage-class为Standard，则该Object直接存储为Standard。

 支持的接口：PutObject、InitMultipartUpload、AppendObject、 PutObjectSymlink、CopyObject

 |

## 细节分析 {#section_zn5_d1x_wdb .section}

-   使用PutSymlink接口创建软链接时不会做以下检查，以下检查会在GetObject等需要访问目标文件的API中进行。
    -   不检查目标文件是否存在。
    -   不检查目标文件类型是否合法。
    -   不检查目标文件是否有权限访问。
-   如果试图添加的文件已经存在，并且有访问权限，新添加的文件将覆盖原来的文件，成功将返回200 OK。
-   使用PutSymlink接口时，携带以x-oss-meta-为前缀的参数，则视为user meta，例如x-oss-meta-location。一个Object可以有多个类似的参数，但所有的user meta总大小不能超过8 KB。

## 示例 {#section_th3_m1x_wdb .section}

请求示例

```
PUT /link-to-oss.jpg?symlink HTTP/1.1 
Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
Cache-control: no-cache 
Content-Disposition: attachment;filename=oss_download.jpg 
Date: Tue, 08 Nov 2016 02:00:25 GMT 
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2PRrk= x-oss-symlink-target: oss.jpg
x-oss-storage-class: Standard
```

返回示例

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Tue, 08 Nov 2016 02:00:25 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 582131B9109F4EE66CDE56A5
ETag: "0A477B89B4602AA8DECB8E19BFD447B6"
```

## SDK {#section_egl_m2c_5gb .section}

PutSymlink接口所对应的各语言SDK如下：

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/管理文件/管理软链接.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/管理文件/管理软链接.md)
-   [PHP](../../../../../cn.zh-CN/SDK 参考/PHP/管理文件/管理软链接.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/管理文件/管理软链接.md)
-   [C](../../../../../cn.zh-CN/SDK 参考/C/管理文件/管理软链接.md)
-   [.NET](../../../../../cn.zh-CN/SDK 参考/.NET/管理文件/管理软链接.md)

## 错误码 {#section_nfp_nfc_5gb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|InvalidArgument|400|StorageClass的值不合法。|

