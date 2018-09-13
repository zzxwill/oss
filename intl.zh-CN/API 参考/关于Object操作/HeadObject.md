# HeadObject {#reference_bgh_cbw_wdb .reference}

HeadObject只返回某个Object的meta信息，不返回文件内容。

## 请求语法 {#section_wmz_jbw_wdb .section}

```
HEAD /ObjectName HTTP/1.1
Host: BucketName/oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求Header {#section_xjh_mbw_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|If-Modified-Since|字符串|如果指定的时间早于实际修改时间，则返回200 OK和Object Meta；否则返回304 not modified 默认值：无

 |
|If-Unmodified-Since|字符串|如果传入参数中的时间等于或者晚于文件实际修改时间，则返回200 OK和Object Meta；否则返回412 precondition failed错误 默认值：无

 |
|If-Match|字符串|如果传入期望的ETag和object的 ETag匹配，则返回200 OK和Object Meta；否则返回412 precondition failed错误默认值：无

|
|If-None-Match|字符串|如果传入的ETag值和Object的ETag不匹配，则返回200 OK和Object Meta；否则返回304 Not Modified 默认值：无

 |

## 细节分析 {#section_bxx_3cw_wdb .section}

-   不论正常返回200 OK还是非正常返回，Head Object都不返回消息体。
-   HeadObject支持在头中设定If-Modified-Since, If-Unmodified-Since, If-Match，If-None-Match的查询条件。具体规则请参见GetObject中对应的选项。如果没有修改，返回304 Not Modified。
-   如果用户在PutObject的时候传入以x-oss-meta-为开头的user meta，比如x-oss-meta-location，返回消息时，返回这些user meta。
-   如果文件不存在返回404 Not Found错误。
-   若该Object为进行服务器端熵编码加密存储的，则在Head请求响应头中，会返回x-oss-server-side-encryption，其值表明该Object的服务器端加密算法。
-   如果文件类型为符号链接， 响应头中`Content-Length`、`ETag`、`Content-Md5` 均为目标文件的元信息；`Last-Modified`是目标文件和符号链接的最大值；其他均为符号链接元信息。
-   如果文件类型为符号链接，并且目标文件不存在，返回404 Not Found错误。错误码：SymlinkTargetNotExist。
-   如果文件类型为符号链接，并且目标文件类型是符号链接，返回400 Bad request错误。错误码：InvalidTargetType。
-   x-oss-storage-class展示Object的存储类型：Standard，IA，Archive。
-   如果Bucket类型为Archive，且用户已经提交Restore请求，则响应头中会以x-oss-restore返回该Object的Restore状态，分如下几种情况：
    -   如果没有提交Restore或者Restore已经超时，则不返回该字段。
    -   如果已经提交Restore，且Restore没有时完成，则返回的x-oss-restore值为: ongoing-request=”true”。
    -   如果已经提交Restore，且Restore已经完成，则返回的x-oss-restore值为: ongoing-request=”false”, expiry-date=”Sun, 16 Apr 2017 08:12:33 GMT”，其中expiry-date是Restore完成后文件进入可读状态的过期时间。

## 示例 {#section_fml_4dw_wdb .section}

**请求示例：**

```
HEAD /oss.jpg HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 07:32:52 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:JbzF2LxZUtanlJ5dLA092wpDC/E=
```

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
x-oss-object-type: Normal
x-oss-storage-class: Archive
Date: Fri, 24 Feb 2012 07:32:52 GMT
Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
ETag: "fba9dede5f27731c9771645a39863328"
Content-Length: 344606
Content-Type: image/jpg
Connection: keep-alive
Server: AliyunOSS
```

**提交Restore请求但restore没有完成时的请求示例**

```
HEAD /oss.jpg HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 07:32:52 GMT
Authorization: OSS e1Unnbm1rgdnpI:KKxkdNrUBu2t1kqlDh0MLbDb99I=
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 58F71A164529F18D7F000045
x-oss-object-type: Normal
x-oss-storage-class: Archive
x-oss-restore: ongoing-request="true"
Date: Sat, 15 Apr 2017 07:32:52 GMT
Last-Modified: Sat, 15 Apr 2017 06:07:48 GMT
ETag: "fba9dede5f27731c9771645a39863328"
Content-Length: 344606
Content-Type: image/jpg
Connection: keep-alive
Server: AliyunOSS
```

**提交Restore请求且restore已经完成时的请求示例**

```
HEAD /oss.jpg HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 09:35:51 GMT
Authorization: OSS e1Unnbm1rgdnpI:21qtGJ+ykDVmdu6O6FMJnn+WuBw=
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 58F725344529F18D7F000055
x-oss-object-type: Normal
x-oss-storage-class: Archive
x-oss-restore: ongoing-request="false", expiry-date="Sun, 16 Apr 2017 08:12:33 GMT"
Date: Sat, 15 Apr 2017 09:35:51 GMT
Last-Modified: Sat, 15 Apr 2017 06:07:48 GMT
ETag: "fba9dede5f27731c9771645a39863328"
Content-Length: 344606
```

