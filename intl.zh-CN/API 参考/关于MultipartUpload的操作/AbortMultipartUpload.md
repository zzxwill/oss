# AbortMultipartUpload {#reference_txp_bvx_wdb .reference}

AbortMultipartUpload接口可以根据用户提供的Upload ID中止其对应的Multipart Upload事件。

当一个Multipart Upload事件被中止后，就不能再使用这个Upload ID做任何操作，已经上传的Part数据也会被删除。

## 请求语法 {#section_w35_2vx_wdb .section}

```
DELETE /ObjectName?uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: Signature
```

## 细节分析 {#section_e4l_yvx_wdb .section}

-   中止一个Multipart Upload事件时，如果其所属的某些Part仍然在上传，那么这次中止操作将无法删除这些Part。所以如果存在并发访问的情况，为了彻底释放OSS上的空间，需要调用几次Abort Multipart Upload接口。
-   如果输入的Upload Id不存在，OSS会返回404错误，错误码为：NoSuchUpload。

## 示例 {#section_lwt_cwx_wdb .section}

**请求示例:**

```
Delete /multipart.data?&uploadId=0004B9895DBBB6EC98E  HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 22 Feb 2012 08:32:21 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:J/lICfXEvPmmSW86bBAfMmUmWjI=
```

**返回示例:**

```
HTTP/1.1 204 
Server: AliyunOSS
Connection: keep-alive
x-oss-request-id: 059a22ba-6ba9-daed-5f3a-e48027df344d
Date: Wed, 22 Feb 2012 08:32:21 GMT
```

