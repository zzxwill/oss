# GetBucketLogging {#reference_mm3_zwv_tdb .reference}

GetBucketLogging接口用于查看存储空间（Bucket）的访问日志配置。只有Bucket的拥有者才能查看Bucket的访问日志配置。

## 请求语法 {#section_cjt_lgw_bz .section}

```
GET /?logging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素 {#section_dvp_mgw_bz .section}

|名称|类型|描述|
|:-|:-|:-|
|BucketLoggingStatus|容器| 访问日志状态信息的容器。

 子元素：LoggingEnabled

 父元素：无

 **说明：** 如果源Bucket未设置日志规则，OSS仍然返回一个XML消息体，但其中的BucketLoggingStatus元素为空。

 |
|LoggingEnabled|容器| 访问日志信息的容器。此元素在开启时返回，关闭时不返回。

 子元素：TargetBucket, TargetPrefix

 父元素：BucketLoggingStatus

 |
|TargetBucket|字符| 指定存放访问日志的Bucket。

 子元素：无

 父元素：BucketLoggingStatus.LoggingEnabled

 |
|TargetPrefix|字符| 指定最终被保存的访问日志文件前缀。

 子元素：无

 父元素：BucketLoggingStatus.LoggingEnabled

 |

## 示例 {#section_sss_pgw_bz .section}

**请求示例**

```
Get /?logging HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 04 May 2012 05:31:04 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=
```

**返回示例（已设置日志规则）**

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

**返回示例（未设置日志规则）**

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

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../intl.zh-CN/SDK 参考/Java/访问日志.md)
-   [Python](../../../../../intl.zh-CN/SDK 参考/Python/访问日志.md)
-   [PHP](../../../../../intl.zh-CN/SDK 参考/PHP/访问日志.md)
-   [Go](../../../../../intl.zh-CN/SDK 参考/Go/设置访问日志.md)
-   [C](../../../../../intl.zh-CN/SDK 参考/C/访问日志.md)
-   [.NET](../../../../../intl.zh-CN/SDK 参考/.NET/访问日志.md)
-   [Node.js](../../../../../intl.zh-CN/SDK 参考/Node.js/访问日志.md)
-   [Ruby](../../../../../intl.zh-CN/SDK 参考/Ruby/设置访问日志.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|AccessDenied|403|没有查看Bucket访问日志配置的权限。只有Bucket的拥有者才能查看Bucket的访问日志配置。|

