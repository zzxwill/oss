# DeleteObject {#reference_iqc_mqv_wdb .reference}

DeleteObject用于删除某个文件（Object）。使用DeleteObject需要对该Object有写权限。

## 请求语法 {#section_dcz_1rv_wdb .section}

```
DELETE /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_lxj_2rv_wdb .section}

如果Object类型为符号链接，使用DeleteObject仅会删除该符号链接。

## 示例 {#section_prc_gsv_wdb .section}

**请求示例**

```
DELETE /AK.txt HTTP/1.1
Host: test.oss-cn-zhangjiakou.aliyuncs.com
Accept-Encoding: identity
User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
Accept: */*
Connection: keep-alive
date: Wed, 02 Jan 2019 13:28:38 GMT
authorization: OSS qn6qrrqxo2oawuk53otfjbyc:zUglwRPGkbByZxm1+y4eyu+NIUs=zV0vhg=
Content-Length: 0
```

**返回示例**

```
HTTP/1.1 204 No Content
Server: AliyunOSS
Date: Wed, 02 Jan 2019 13:28:38 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5C2CBC8653718B5511EF4535
x-oss-server-time: 134
```

## SDK {#section_egl_m2c_5gb .section}

DeleteObject接口所对应的各语言SDK如下：

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/管理文件/删除文件.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/管理文件/删除文件.md)
-   [PHP](../../../../../cn.zh-CN/SDK 参考/Python/管理文件/删除文件.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/管理文件/删除文件.md)
-   [C](../../../../../cn.zh-CN/SDK 参考/C/管理文件/删除文件.md)
-   [.NET](../../../../../cn.zh-CN/SDK 参考/.NET/管理文件/删除文件.md)
-   [Android](../../../../../cn.zh-CN/SDK 参考/Android/管理文件/删除文件.md)
-   [iOS](../../../../../cn.zh-CN/SDK 参考/iOS/管理文件.md)
-   [Node.js](../../../../../cn.zh-CN/SDK 参考/Node.js/管理文件.md)
-   [Browser.js](../../../../../cn.zh-CN/SDK 参考/Browser.js/管理文件.md)
-   [Ruby](../../../../../cn.zh-CN/SDK 参考/Ruby/管理文件.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|Not Found|404|目标Bucket不存在。|
|No Content|204|目标Object不存在。|

