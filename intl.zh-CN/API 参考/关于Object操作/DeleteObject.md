# DeleteObject {#reference_iqc_mqv_wdb .reference}

DeleteObject用于删除某个Object。

## 请求语法 {#section_dcz_1rv_wdb .section}

```
DELETE /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_lxj_2rv_wdb .section}

-   DeleteObject要求对该Object要有写权限。
-   如果要删除的Object不存在，OSS也返回状态码204（ No Content）。
-   如果Bucket不存在，返回404 Not Found。
-   如果文件类型为**符号链接**，只删除符号链接自身。

## 示例 {#section_prc_gsv_wdb .section}

**请求示例：**

```
DELETE /copy_oss.jpg HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 07:45:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:zUglwRPGkbByZxm1+y4eyu+NIUs=
```

**返回示例：**

```
HTTP/1.1 204 NoContent
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Fri, 24 Feb 2012 07:45:28 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

