# AbortMultipartUpload {#reference_txp_bvx_wdb .reference}

AbortMultipartUpload接口用于终止MultipartUpload事件。您需要提供MultipartUpload事件相应的Upload ID。

**说明：** 

-   当一个MultipartUpload事件被中止后，您无法再使用这个Upload ID做任何操作，已经上传的Part数据也会被删除。
-   中止一个MultipartUpload事件时，如果其所属的某些Part仍然在上传，那么这次中止操作将无法删除这些Part。所以如果存在并发访问的情况，需要调用几次AbortMultipartUpload接口以彻底释放OSS上的空间。

## 请求语法 {#section_w35_2vx_wdb .section}

```
DELETE /ObjectName?uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: Signature
```

## 示例 {#section_lwt_cwx_wdb .section}

**请求示例**

```
Delete /multipart.data?&uploadId=0004B9895DBBB6EC98E  HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 22 Feb 2012 08:32:21 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:J/lICfXEvPmmSW86bBAfMmUmWjI=
```

**返回示例**

```
HTTP/1.1 204 
Server: AliyunOSS
Connection: keep-alive
x-oss-request-id: 059a22ba-6ba9-daed-5f3a-e48027df344d
Date: Wed, 22 Feb 2012 08:32:21 GMT
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../intl.zh-CN/SDK 参考/Java/上传文件/分片上传.md)
-   [PHP](../../../../../intl.zh-CN/SDK 参考/PHP/上传文件/分片上传.md)
-   [Go](../../../../../intl.zh-CN/SDK 参考/Go/上传文件/分片上传.md)
-   [C](../../../../../intl.zh-CN/SDK 参考/Go/上传文件/分片上传.md)
-   [.NET](../../../../../intl.zh-CN/SDK 参考/.NET/上传文件/分片上传.md)

## 错误码 {#section_nfp_nfc_5gb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchUpload|404|Upload ID不存在。|

