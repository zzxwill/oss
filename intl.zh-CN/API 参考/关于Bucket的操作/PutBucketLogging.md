# PutBucketLogging {#reference_t1g_zj5_tdb .reference}

PutBucketLogging接口用于为 Bucket 开启访问日志记录功能。

访问日志记录功能开启后，OSS 将自动记录访问这个 Bucket 请求的详细信息，并按照用户指定的规则，以小时为单位，将访问日志作为一个 Object 写入用户指定的 Bucket。

**说明：** OSS 提供 Bucket 访问日志的目的是为了方便 Bucket 的拥有者理解和分析 Bucket 的访问行为。OSS 提供的 Bucket 访问日志不保证记录每一条访问信息。

## 请求语法 {#section_rfs_rnr_bz .section}

```
PUT /?logging HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Authorization: SignatureValue 
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
<?xml version="1.0" encoding="UTF-8"?>
<BucketLoggingStatus>
    <LoggingEnabled>
        <TargetBucket>TargetBucket</TargetBucket>
        <TargetPrefix>TargetPrefix</TargetPrefix>
    </LoggingEnabled>
</BucketLoggingStatus>
```

## 请求元素\(Request Elements\) {#section_dwh_vnr_bz .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|BucketLoggingStatus|容器|是|访问日志状态信息的容器。子元素：LoggingEnabled

父元素：无

|
|LoggingEnabled|容器|否|访问日志信息的容器。这个元素在开启时需要，关闭时不需要。子元素：TargetBucket, TargetPrefix

父元素：BucketLoggingStatus

|
|TargetBucket|字符|在开启访问日志的时候必需|指定存储访问日志的 Bucket。子元素：无

父元素：BucketLoggingStatus.LoggingEnabled

|
|TargetPrefix|字符|否|指定最终被保存的访问日志文件前缀。子元素：无

父元素：BucketLoggingStatus.LoggingEnabled

|

## 存储访问日志记录的 Object 命名规则 {#section_kv3_ynr_bz .section}

```
<TargetPrefix><SourceBucket>-YYYY-mm-DD-HH-MM-SS-UniqueString
```

命名规则中，TargetPrefix 由用户指定；YYYY、mm、DD、HH、MM和SS分别是该 Object 被创建时的阿拉伯数字的年、月、日、小时、分钟和秒（注意位数）；UniqueString 为 OSS 系统生成的字符串。

一个实际用于存储 OSS 访问日志的 Object 名称示例 如下：

```
MyLog-oss-example-2012-09-10-04-00-00-0000
```

上述示例中，MyLog- 表示用户指定的 Object 前缀；oss-example 表示源 Bucket 的名称；2012-09-10-04-00-00表示该 Object 被创建时的北京时间；0000表示 OSS 系统生成的字符串。

## LOG文件格式 {#section_ugs_b4r_bz .section}

|名称|例子|含义|
|--|--|--|
|Remote IP|119.140.142.11|请求发起的IP地址（Proxy 代理或用户防火墙可能会屏蔽该字段）|
|Reserved|-|保留字段|
|Reserved|-|保留字段|
|Time|\[02/May/2012:00:00:04 +0800\]|OSS 收到请求的时间|
|Request-URL|GET /aliyun-logo.png HTTP/1.1|用户请求的 URL （包括 query-string）|
|HTTP Status|200|OSS 返回的 HTTP 状态码|
|SentBytes|5576|用户从 OSS 下载的流量|
|RequestTime \(ms\)|71|完成本次请求的时间（毫秒）|
|Referer| ```
http://www.aliyun.com/product/oss
```

 |请求的 HTTP Referer|
|User-Agent|curl/7.15.5|HTTP 的 User-Agent header|
|HostName|oss-example.regionid.example.com|请求访问域名|
|Request ID|505B01695037C2AF032593A4|用于唯一标示该请求的 UUID|
|LoggingFlag|true|是否开启了访问日志功能|
|Requester Aliyun ID|1657136103983691|请求者的阿里云ID；匿名访问为 “-”|
|Operation|GetObject|请求类型|
|Bucket|oss-example|请求访问的 Bucket 名称|
|Key|/aliyun-logo.png|用户请求的 Key|
|ObjectSize|5576|Object 大小|
|Server Cost Time \(ms\)|17|OSS 服务器处理本次请求所花的时间（毫秒）|
|Error Code|NoSuchBucket|OSS 返回的错误码|
|Request Length|302|用户请求的长度（Byte）|
|UserID|1657136103983691|Bucket 拥有者 ID|
|Delta DataSize|280|Bucket 大小的变化量；若没有变化为 “-”|
|Sync Request|-|是否是 CDN 回源请求；若不是为 “-”|
|Reserved|-|保留字段|

## 细节分析 {#section_zdp_24r_bz .section}

-   源 Bucket 和目标 Bucket 必须属于同一用户。如果源 Bucket 不存在，OSS返回错误码：NoSuchBucket。
-   源 Bucket 和目标 Bucket 必须属于同一个数据中心，否则返回400错误，错误码为：InvalidTargetBucketForLogging。
-   源 Bucket 和目标 Bucket 可以是同一个Bucket，也可以是不同的 Bucket；用户也可以将多个源 Bucket 的 LOG 都保存在同一个目标 Bucket 内（建议指定不同的 TargetPrefix）。
-   源 Bucket 被删除时，对应的 Logging 规则也将被删除。
-   上面所示的请求语法中，“BucketName” 表示要开启访问日志记录的 Bucket；“TargetBucket” 表示访问日志记录要存入的 Bucket；“TargetPrefix” 表示存储访问日志记录的 Object 名称前缀，可以为空。
-   当关闭一个 Bucket 的访问日志记录功能时，只要发送一个空的 BucketLoggingStatus 即可，具体方法可以参考下面的请求示例。
-   如果用户上传了 Content-MD5 请求头，OSS 会计算 Body 的 Content-MD5 并检查一致性，如果不一致，将返回 InvalidDigest 错误码。
-   -   PUT Bucket Logging
    -   所有 PUT Bucket Logging 请求必须带签名，即不支持匿名访问。
    -   如果 PUT Bucket Logging 请求发起者不是源 Bucket（请求示例中的 BucketName）的拥有者，OSS 返回403错误码。
    -   如果 PUT Bucket Logging 请求发起者不是目标 Bucket（请求示例中的 TargetBucket）的拥有者，OSS 返回403；如果目标 Bucket 不存在，OSS 返回错误码：InvalidTargetBucketForLogging。
    -   如果 PUT Bucket Logging 请求中的XML不合法，返回错误码：MalformedXML。
-   OSS LOG
    -   OSS 以小时为单位生成 Bucket 访问的 LOG 文件，但并不表示这个小时的所有请求都记录在这个小时的 LOG 文件内，也有可能出现在上一个或者下一个LOG文件中。
    -   OSS 生成的 LOG 文件命名规则中的 “UniqueString” 仅仅是 OSS 为其生成的 UUID，用于唯一标识该文件。
    -   OSS 生成一个 Bucket 访问的 LOG 文件，算作一次 PUT 操作，并记录其占用的空间，但不会记录产生的流量。LOG 生成后，用户可以按照普通的 Object 来操作这些 LOG 文件。
    -   OSS 会忽略掉所有以 “x-” 开头的 query-string 参数，但这个 query-string 会被记录在访问 LOG 中。如果你想从海量的访问日志中，标示一个特殊的请求，可以在 URL 中添加一个 “x-” 开头的 query-string 参数。如：

        `http://oss-example.regionid.example.com/aliyun-logo.png`

        `http://oss-example.regionid.example.com/aliyun-logo.png?x-user=admin`

        OSS 处理上面两个请求，结果是一样的。但是在访问 LOG 中，你可以通过搜索 “x-user=admin”，很方便地定位出经过标记的这个请求。

    -   OSS 的 LOG 中的任何一个字段，都可能出现 “-”，用于表示未知数据或对于当前请求该字段无效。
    -   根据需求，OSS 的 LOG 格式将来会在尾部添加一些字段，请开发者开发 Log 处理工具时考虑兼容性的问题。

## 示例 {#section_byq_m4r_bz .section}

开启 Bucket 访问日志的请求示例：

```
PUT /?logging HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length: 186
Date: Fri, 04 May 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
<?xml version="1.0" encoding="UTF-8"?>
<BucketLoggingStatus>
<LoggingEnabled>
<TargetBucket>doc-log</TargetBucket>
<TargetPrefix>MyLog-</TargetPrefix>
</LoggingEnabled>
</BucketLoggingStatus>
```

返回示例：

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

关闭 Bucket 访问日志的请求示例：

```
PUT /?logging HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Type: application/xml
Content-Length: 86
Date: Fri, 04 May 2012 04:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
<?xml version="1.0" encoding="UTF-8"?>
<BucketLoggingStatus>
</BucketLoggingStatus>
```

返回示例：

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 04:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

