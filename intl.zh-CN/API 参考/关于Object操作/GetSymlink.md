# GetSymlink {#reference_s3d_s1x_wdb .reference}

GetSymlink用于获取符号链接，此操作要求用户对该符号链接有读权限。

## 请求语法 {#section_czv_w1x_wdb .section}

```
GET /ObjectName?symlink HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应Header {#section_pwz_y1x_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|x-oss-symlink-target|字符串|符号链接指向的目标文件。|

## 细节分析 {#section_iq4_fbx_wdb .section}

如果符号链接不存在返回404 Not Found错误。错误码：NoSuchKey

## 示例 {#section_jcp_hbx_wdb .section}

**请求示例：**

```
GET /link-to-oss.jpg?symlink HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 06:38:30 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:UNQDb7GapEgJCZkcde6OhZ9Jfe8=
```

**返回示例：**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Fri, 24 Feb 2012 06:38:30 GMT
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
Content-Length: 0
Connection: keep-alive
x-oss-request-id: 5650BD72207FB30443962F9A
x-oss-symlink-target: oss.jpg
ETag: "A797938C31D59EDD08D86188F6D5B872"
```

