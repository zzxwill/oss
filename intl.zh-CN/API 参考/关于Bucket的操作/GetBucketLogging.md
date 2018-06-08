# GetBucketLogging {#reference_mm3_zwv_tdb .reference}

GetBucketLogging用于查看Bucket的访问日志配置情况。

## 请求语法 {#section_cjt_lgw_bz .section}

```
GET /?logging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素\(Response Elements\) {#section_dvp_mgw_bz .section}

|名称|类型|描述|
|--|--|--|
|BucketLoggingStatus|容器|访问日志状态信息的容器。子元素：LoggingEnabled

父元素：无

|
|LoggingEnabled|容器|访问日志信息的容器。这个元素在开启时需要，关闭时不需要。子元素：TargetBucket, TargetPrefix

父元素：BucketLoggingStatus

|
|TargetBucket|字符|指定存放访问日志的Bucket。子元素：无

父元素：BucketLoggingStatus.LoggingEnabled

|
|TargetPrefix|字符|指定最终被保存的访问日志文件前缀。子元素：无

父元素：BucketLoggingStatus.LoggingEnabled

|

## 细节分析 {#section_o5y_4gw_bz .section}

-   如果Bucket不存在，返回404 no content错误。错误码：NoSuchBucket。
-   只有Bucket的拥有者才能查看Bucket的访问日志配置情况，否则返回403 Forbidden错误，错误码：AccessDenied。
-   如果源Bucket未设置Logging规则，OSS仍然返回一个XML消息体，但其中的BucketLoggingStatus元素为空。

## 示例 {#section_sss_pgw_bz .section}

**请求示例：**

```
Get /?logging HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 04 May 2012 05:31:04 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=
```

**已设置LOG规则的返回示例：**

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 05:31:04 GMT
Connection: keep-alive
Content-Length: 210  
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<BucketLoggingStatus xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <LoggingEnabled>
        <TargetBucket>mybucketlogs</TargetBucket>
        <TargetPrefix>mybucket-access_log/</TargetPrefix>
    </LoggingEnabled>
</BucketLoggingStatus>
```

**未设置LOG规则的返回示例：**

```
HTTP/1.1 200 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 04 May 2012 05:31:04 GMT
Connection: keep-alive
Content-Length: 110  
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<BucketLoggingStatus xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
</BucketLoggingStatus>
```

