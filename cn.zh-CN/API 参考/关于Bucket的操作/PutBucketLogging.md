# PutBucketLogging {#reference_t1g_zj5_tdb .reference}

PutBucketLogging接口用于为bucket开启访问日志记录功能。

这个功能开启后，OSS将自动记录访问这个bucket请求的详细信息，并按照用户指定的规则，以小时为单位，将访问日志作为一个Object写入用户指定的bucket。OSS提供Bucket访问日志的目的是为了方便bucket的拥有者理解和分析bucket的访问行为。OSS提供的Bucket访问日志不保证记录下每一条访问记录。

**说明：** OSS提供Bucket访问日志的目的是为了方便bucket的拥有者理解和分析bucket的访问行为。OSS提供的Bucket访问日志不保证记录下每一条访问记录。

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
|BucketLoggingStatus|容器|是|访问日志状态信息的容器子元素：LoggingEnabled

父元素：无

|
|LoggingEnabled|容器|否|访问日志信息的容器。这个元素在开启时需要，关闭时不需要。子元素：TargetBucket, TargetPrefix

父元素：BucketLoggingStatus

|
|TargetBucket|字符|在开启访问日志的时候必需|指定存放访问日志的Bucket。子元素：无

父元素：BucketLoggingStatus.LoggingEnabled

|
|TargetPrefix|字符|否|指定最终被保存的访问日志文件前缀。类型：子元素：None父元素：BucketLoggingStatus.LoggingEnabled|

## 存储访问日志记录的object命名规则 {#section_kv3_ynr_bz .section}

```
<TargetPrefix><SourceBucket>-YYYY-mm-DD-HH-MM-SS-UniqueString
```

命名规则中，TargetPrefix由用户指定；YYYY, mm, DD, HH, MM和SS分别是该Object被创建时的阿拉伯数字的年，月，日，小时，分钟和秒（注意位数）；UniqueString为OSS系统生成的字符串。一个实际的用于存储OSS访问日志的Object名称例子如下：

```
MyLog-oss-example-2012-09-10-04-00-00-0000
```

上例中，“MyLog-”是用户指定的Object前缀；“oss-example”是源bucket的名称；“2012-09-10-04-00-00”是该Object被创建时的北京时间；“0000” 是OSS系统生成的字符串。

## LOG文件格式 {#section_ugs_b4r_bz .section}

|名称|例子|含义|
|--|--|--|
|Remote IP|119.140.142.11|请求发起的IP地址（Proxy代理或用户防火墙可能会屏蔽该字段）|
|Reserved|-|保留字段|
|Reserved|-|保留字段|
|Time|\[02/May/2012:00:00:04 +0800\]|OSS收到请求的时间|
|Request-URI|“GET /aliyun-logo.png HTTP/1.1“|用户请求的URI\(包括query-string\)|
|HTTP Status|200|OSS返回的HTTP状态码|
|SentBytes|5576|用户从OSS下载的流量|
|RequestTime \(ms\)|71|完成本次请求的时间（毫秒）|
|Referer| ```
http://www.aliyun.com/product/oss
```

 |请求的HTTP Referer|
|User-Agent|curl/7.15.5|HTTP的User-Agent头|
|HostName|oss-example.regionid.example.com|请求访问域名|
|Request ID|505B01695037C2AF032593A4|用于唯一标示该请求的UUID|
|LoggingFlag|true|是否开启了访问日志功能|
|Requester Aliyun ID|1657136103983691|请求者的阿里云ID；匿名访问为“-”|
|Operation|GetObject|请求类型|
|Bucket|oss-example|请求访问的Bucket名字|
|Key|/aliyun-logo.png|用户请求的Key|
|ObjectSize|5576|Object大小|
|Server Cost Time \(ms\)|17|OSS服务器处理本次请求所花的时间（毫秒）|
|Error Code|NoSuchBucket|OSS返回的错误码|
|Request Length|302|用户请求的长度（Byte）|
|UserID|1657136103983691|Bucket拥有者ID|
|Delta DataSize|280|Bucket大小的变化量；若没有变化为“-”|
|Sync Request|-|是否是CDN回源请求；若不是为“-”|
|Reserved|-|保留字段|

## 细节分析 {#section_zdp_24r_bz .section}

-   源Bucket和目标Bucket必须属于同一个用户。
-   上面所示的请求语法中，“BucketName”表示要开启访问日志记录的bucket；“TargetBucket”表示访问日志记录要存入的bucket；“TargetPrefix”表示存储访问日志记录的object名字前缀，可以为空。
-   源bucket和目标bucket可以是同一个Bucket，也可以是不同的Bucket；用户也可以将多个的源bucket的LOG都保存在同一个目标bucket内（建议指定不同的TargetPrefix）。
-   当关闭一个Bucket的访问日志记录功能时，只要发送一个空的BucketLoggingStatus即可，具体方法可以参考下面的请求示例。
-   所有PUT Bucket Logging请求必须带签名，即不支持匿名访问。
-   如果PUT Bucket Logging请求发起者不是源bucket（请求示例中的BucketName）的拥有者，OSS返回403错误码。
-   如果源bucket不存在，OSS返回错误码：NoSuchBucket。
-   如果PUT Bucket Logging请求发起者不是目标bucket（请求示例中的TargetBucket）的拥有者，OSS返回403；如果目标bucket不存在，OSS返回错误码：InvalidTargetBucketForLogging。
-   源Bucket和目标Bucket必须属于同一个数据中心，否则返回400错误，错误码为：InvalidTargetBucketForLogging。
-   PUT Bucket Logging请求中的XML不合法，返回错误码：MalformedXML。
-   源bucket和目标bucket可以是同一个Bucket；用户也可以将不同的源bucket的LOG都保存在同一个目标bucket内（注意要指定不同的TargetPrefix）。
-   源Bucket被删除时，对应的Logging规则也将被删除。
-   OSS以小时为单位生成bucket访问的Log文件，但并不表示这个小时的所有请求都记录在这个小时的LOG文件内，也有可能出现在上一个或者下一个LOG文件中。
-   OSS生成的Log文件命名规则中的“UniqueString”仅仅是OSS为其生成的UUID，用于唯一标识该文件。
-   OSS生成一个bucket访问的Log文件，算作一次PUT操作，并记录其占用的空间，但不会记录产生的流量。LOG生成后，用户可以按照普通的Object来操作这些LOG文件。
-   OSS会忽略掉所有以“x-”开头的query-string参数，但这个query-string会被记录在访问LOG中。如果你想从海量的访问日志中，标示一个特殊的请求，可以在URL中添加一个“x-”开头的query-string参数。如：

    `http://oss-example.regionid.example.com/aliyun-logo.png`

    `http://oss-example.regionid.example.com/aliyun-logo.png?x-user=admin`

    OSS处理上面两个请求，结果是一样的。但是在访问LOG中，你可以通过搜索“x-user=admin”，很方便地定位出经过标记的这个请求。

-   OSS的LOG中的任何一个字段，都可能出现“-”，用于表示未知数据或对于当前请求该字段无效。
-   根据需求，OSS的LOG格式将来会在尾部添加一些字段，请开发者开发Log处理工具时考虑兼容性的问题。
-   如果用户上传了Content-MD5请求头，OSS会计算body的Content-MD5并检查一致性，如果不一致，将返回InvalidDigest错误码。

## 示例 {#section_byq_m4r_bz .section}

**开启bucket访问日志的请求示例：**

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

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 03:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

**关闭bucket访问日志的请求示例：**

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

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 04:21:12 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

