# OSS错误响应 {#concept_dt2_hq3_wdb .concept}

当用户访问OSS出现错误时，OSS会返回给用户相应的错误码和错误信息，便于用户定位问题，并做出适当的处理。

## OSS的错误响应格式 {#section_zgc_wq3_wdb .section}

当用户访问OSS出错时，OSS会返回给用户一个合适的3xx、4xx或者5xx的HTTP状态码，以及一个application/xml格式的消息体。

错误响应的消息体例子：

```
<?xml version="1.0" ?>
<Error xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Code>
        AccessDenied
    </Code>
    <Message>
        Query-string authentication requires the Signature, Expires and OSSAccessKeyId parameters
    </Message>
    <RequestId>
        1D842BC5425544BB
    </RequestId>
    <HostId>
        oss-cn-hangzhou.aliyuncs.com
    </HostId>
</Error>
```

所有错误的消息体中都包括以下几个元素：

-   Code：OSS返回给用户的错误码。
-   Message：OSS给出的详细错误信息。
-   RequestId：用于唯一标识该次请求的UUID。当你无法解决问题时，可以凭这个RequestId来请求OSS开发工程师的帮助。
-   HostId：用于标识访问的OSS集群，与用户请求时使用的Host一致。

其他特殊的错误信息元素请参照每个请求的具体介绍。

## OSS的错误码 {#section_g4x_wq3_wdb .section}

OSS的错误码列表如下：

|错误码|描述|HTTP状态码|说明|
|:--|:-|:------|:-|
|AccessDenied|拒绝访问|403|原因及排除请参看[权限问题及排查](cn.zh-CN/常见错误排除/OSS 权限问题及排查.md#)|
|BucketAlreadyExists|Bucket已经存在|409|[CreateBucket](../../../../cn.zh-CN/API 参考/关于Bucket的操作/PutBucket.md#)指定的BucketName已经使用，请选择新的[BucketName](../../../../cn.zh-CN/开发指南/基本概念介绍.md#)|
|BucketNotEmpty|Bucket不为空|409|[DeleteBucket](../../../../cn.zh-CN/API 参考/关于Bucket的操作/DeleteBucket.md#)前请先删除文件和未完成的分片上传任务|
|CallbackFailed|上传回调失败|203|原因及排除请参看[上传回调错误及排除](cn.zh-CN/常见错误排除/上传回调错误及排除.md#)|
|EntityTooLarge|实体过大|400|Post请求消息长度超过 5GB，原因及排除请参看[PostObject错误及排查](cn.zh-CN/常见错误排除/Post Object错误及排查.md#)|
|EntityTooSmall|实体过小|400|Post请求消息长度太短，排除请参看PostObject错误及排查|
|FieldItemTooLong|Post请求中表单域过大|400|除了`file`的表单域大小不要超过4KB，排除请参看PostObject错误及排查|
|FilePartInterity|文件Part已改变|400|读分片数据时发现数据与校验和不符|
|FilePartNotExist|文件Part不存在|400|[CompleteMultipartUpload](../../../../cn.zh-CN/API 参考/关于MultipartUpload的操作/CompleteMultipartUpload.md#)提交的分片没有上传|
|FilePartStale|文件Part过时|400|读分片数据时发现数据与长度不符|
|IncorrectNumberOfFilesInPOSTRequest|Post请求中文件个数非法|400|Post请求表单域中只能有一个`file`域，排除请参看PostObject错误及排查|
|InvalidArgument|参数格式错误|400|参数格式不符合要求，请对照相应[API](../../../../cn.zh-CN/API 参考/API概览.md#)|
|InvalidAccessKeyId|AccessKeyId不存在|403|AccessKeyId无效或过期，排除请参看[403错误及排查](cn.zh-CN/常见错误排除/OSS 403错误及排查.md#)|
|InvalidBucketName|无效的Bucket名字|400|Bucket命名规则请参看[开发人员指南](../../../../cn.zh-CN/开发指南/基本概念介绍.md#)|
|InvalidDigest|无效的摘要|400|指定的MD5校验值与文件不符，MD5的计算方法请参见[PutObject](../../../../cn.zh-CN/API 参考/关于Object操作/PutObject.md#)|
|InvalidEncryptionAlgorithmError|指定的熵编码加密算法错误|400|目前只支持`AES256`加密算法，详见[PutObject](../../../../cn.zh-CN/API 参考/关于Object操作/PutObject.md#)|
|InvalidObjectName|无效的Object名字|400|ObjectName命名规则请参看[开发人员指南](../../../../cn.zh-CN/开发指南/基本概念介绍.md#)|
|InvalidPart|无效的Part|400|[CompleteMultipartUpload](../../../../cn.zh-CN/API 参考/关于MultipartUpload的操作/CompleteMultipartUpload.md#)提交的Part无效，`PartNumber`或`ETag`错误|
|InvalidPartOrder|无效的part顺序|400|[CompleteMultipartUpload](../../../../cn.zh-CN/API 参考/关于MultipartUpload的操作/CompleteMultipartUpload.md#)提交的Part需按照`PartNumber`升序排列|
|InvalidPolicyDocument|无效的Policy文档|400|Post请求中`Policy`无效，排除请参看[PostObject错误及排查](cn.zh-CN/常见错误排除/Post Object错误及排查.md#)|
|InvalidTargetBucketForLogging|Logging操作中有无效的目标bucket|400|存放[Logging](../../../../cn.zh-CN/API 参考/关于Bucket的操作/PutBucketLogging.md#)的目标bucket不存在，请更换|
|InternalError|OSS内部发生错误|500|请重试|
|MalformedXML|XML格式非法|400|请求中`XML`非法，请根据具体请求排除[DeleteObjects](../../../../cn.zh-CN/API 参考/关于Object操作/DeleteMultipleObjects.md#)、[CompleteMultipartUpload](../../../../cn.zh-CN/API 参考/关于MultipartUpload的操作/CompleteMultipartUpload.md#)、[PutBucketLogging](../../../../cn.zh-CN/API 参考/关于Bucket的操作/PutBucketLogging.md#)、[PutBucketWebsite](../../../../cn.zh-CN/API 参考/关于Bucket的操作/PutBucketWebsite.md#)、[PutBucketLifecycle](../../../../cn.zh-CN/API 参考/关于Bucket的操作/PutBucketLifecycle.md#)、[PutBucketReferer](../../../../cn.zh-CN/API 参考/关于Bucket的操作/PutBucketReferer.md#)、[PutBucketCORS](../../../../cn.zh-CN/API 参考/跨域资源共享/PutBucketcors.md#)|
|MalformedPOSTRequest|Post请求的body格式非法|400|表单域格式非法，排除请参看[PostObject错误及排查](cn.zh-CN/常见错误排除/Post Object错误及排查.md#)|
|MaxPOSTPreDataLengthExceededError|Post请求上传文件内容之外的body过大|400|除了`file`的表单域大小不要超过4KB，排除请参看[PostObject错误及排查](cn.zh-CN/常见错误排除/Post Object错误及排查.md#)|
|MethodNotAllowed|不支持的方法|405|以OSS不支持的操作来访问资源|
|MissingArgument|缺少参数|411|请参看具体的[API](../../../../cn.zh-CN/API 参考/API概览.md#)对照解决|
|MissingContentLength|缺少内容长度|411|消息即非[chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1)又没有携带`Content-Length`|
|NoSuchBucket|Bucket不存在|404| |
|NoSuchKey|Object不存在|404| |
|NoSuchUpload|Multipart Upload ID不存在|404|没有初始化分片上传或者初始化的分片上传过期|
|NotImplemented|无法处理的方法|400|OSS不支持的操作|
|ObjectNotAppendable|不是可追加文件|409|OSS有三类文件[normal](../../../../cn.zh-CN/API 参考/关于Object操作/PutObject.md#)、[appendable](../../../../cn.zh-CN/API 参考/关于Object操作/AppendObject.md#)、[multipart](../../../../cn.zh-CN/API 参考/关于MultipartUpload的操作/InitiateMultipartUpload.md#)，只有`appendable`类型的文件才能执行`AppendObject`|
|PositionNotEqualToLength|Append的位置和文件长度不相等|409|详见[AppendObject](../../../../cn.zh-CN/API 参考/关于Object操作/AppendObject.md#)|
|PreconditionFailed|预处理错误|412|下载条件不符合，详见[GetObject](../../../../cn.zh-CN/API 参考/关于Object操作/GetObject.md#)|
|RequestTimeTooSkewed|发起请求的时间和服务器时间超出15分钟|403|排除请参看[403错误及排查](cn.zh-CN/常见错误排除/OSS 403错误及排查.md#)|
|RequestTimeout|请求超时|400|排除请参看[网络超时处理](cn.zh-CN/常见错误排除/网络超时处理.md#)|
|RequestIsNotMultiPartContent|Post请求content-type非法|400|排除请参看[PostObject错误及排查](cn.zh-CN/常见错误排除/Post Object错误及排查.md#)|
|DownloadTrafficRateLimitExceeded|下载流量超过限制|503| 默认上限是 5Gbps，包括内外网，有调整需求请[提交工单](https://workorder.console.aliyun.com/#/ticket/createIndex)

 |
|UploadTrafficRateLimitExceeded|上传流量超过限制|503| 默认上限是 5Gbps，包括内外网，有调整需求请[提交工单](https://workorder.console.aliyun.com/#/ticket/createIndex)

 |
|SignatureDoesNotMatch|签名错误|403|排除请参看[Header中签名](../../../../cn.zh-CN/API 参考/访问控制/在Header中包含签名.md#)、[URL中签名](../../../../cn.zh-CN/API 参考/访问控制/在URL中包含签名.md#)|
|TooManyBuckets|Bucket数目超过限制|400| 默认上限是 10，有调整需求请[提交工单](https://workorder.console.aliyun.com/#/ticket/createIndex)

 |

OSS常见错误及排除

-   [上传回调错误及排除](cn.zh-CN/常见错误排除/上传回调错误及排除.md#)
-   [403错误及排查](cn.zh-CN/常见错误排除/OSS 403错误及排查.md#)
-   [PostObject错误及排查](cn.zh-CN/常见错误排除/Post Object错误及排查.md#)
-   [权限问题及排查](cn.zh-CN/常见错误排除/OSS 权限问题及排查.md#)
-   [跨域资源共享CORS错误及排除](cn.zh-CN/常见错误排除/OSS跨域资源共享（CORS）错误及排除.md#)
-   [防盗链Referer配置及错误排除](cn.zh-CN/常见错误排除/OSS防盗链（Referer）配置及错误排除.md#)
-   [STS常见问题及排查](cn.zh-CN/常见错误排除/STS常见问题及排查.md#)

SDK/Tool常见错误及排除

-   [Java SDK](https://help.aliyun.com/document_detail/32024.html)
-   [Python SDK](https://help.aliyun.com/document_detail/32039.html)
-   [C SDK](https://help.aliyun.com/document_detail/32142.html)
-   [ossfs](../../../../cn.zh-CN/常用工具/ossfs/FAQ.md#)
-   [ossftp](../../../../cn.zh-CN/常用工具/ossftp/如何快速安装OSS FTP.md#)

## OSS不支持的操作 {#section_ecd_wr3_wdb .section}

如果试图以OSS不支持的操作来访问某个资源，返回405 Method Not Allowed错误。

错误请求示例：

```
ABC /1.txt HTTP/1.1
Host: bucketname.oss-cn-shanghai.aliyuncs.com
Date: Thu, 11 Aug 2016 03:53:40 GMT
Authorization: signatureValue
```

返回示例：

```
HTTP/1.1 405 Method Not Allowed
Server: AliyunOSS
Date: Thu, 11 Aug 2016 03:53:44 GMT
Content-Type: application/xml
Content-Length: 338
Connection: keep-alive
x-oss-request-id: 57ABF6C8BC4D25D86CBA5ADE
Allow: GET DELETE HEAD PUT POST OPTIONS
<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>MethodNotAllowed</Code>
  <Message>The specified method is not allowed against this resource.</Message>
  <RequestId>57ABF6C8BC4D25D86CBA5ADE</RequestId>
  <HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
  <Method>abc</Method>
  <ResourceType>Bucket</ResourceType>
</Error>
```

**说明：** 如果访问的资源是 /bucket/， ResourceType应该是bucket。如果访问的资源是 /bucket/object，ResourceType应该是object。

## OSS操作支持但参数不支持的操作 {#section_hrc_fs3_wdb .section}

如果在OSS合法的操作中，添加了OSS不支持的参数（例如在PUT的时候，加入If-Modified-Since参数），OSS会返回400 Bad Request错误。

错误请求示例：

```
PUT /abc.zip HTTP/1.1
Host: bucketname.oss-cn-shanghai.aliyuncs.com
Accept: */*
Date: Thu, 11 Aug 2016 01:44:50 GMT
If-Modified-Since: Thu, 11 Aug 2016 01:43:51 GMT
Content-Length: 363
```

返回示例：

```
HTTP/1.1 400 Bad Request
Server: AliyunOSS
Date: Thu, 11 Aug 2016 01:44:54 GMT
Content-Type: application/xml
Content-Length: 322
Connection: keep-alive
x-oss-request-id: 57ABD896CCB80C366955187E
x-oss-server-time: 0
<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>NotImplemented</Code>
  <Message>A header you provided implies functionality that is not implemented.</Message>
  <RequestId>57ABD896CCB80C366955187E</RequestId>
  <HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
  <Header>If-Modified-Since</Header>
</Error>
```

