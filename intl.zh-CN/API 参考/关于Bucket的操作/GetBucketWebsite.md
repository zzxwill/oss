# GetBucketWebsite {#reference_wvy_s4w_tdb .reference}

GetBucketWebsite操作用于查看Bucket的静态网站托管状态。

## 请求语法 {#section_jbk_tgw_bz .section}

```
GET /?website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素\(Response Elements\) {#section_aqn_ygw_bz .section}

|名称|类型|描述|
|--|--|--|
|ErrorDocument|容器|子元素Key的父元素父元素：WebsiteConfiguration

|
|IndexDocument|容器|子元素Suffix的父元素父元素：WebsiteConfiguration

|
|Key|字符串|返回404错误时使用的文件名 父元素：WebsiteConfiguration.ErrorDocument

有条件：当ErrorDocument设置时，必需

|
|Suffix|字符串|返回目录URL时添加的索引文件名，不要为空，也不要包含"/"。例如索引文件设置为index.html，则访问：regionid.example.com/mybucket/mydir/这样请求的时候默认都相当于访问regionid.example.com/mybucket/index.html 父元素：WebsiteConfiguration.IndexDocument

|
|WebsiteConfiguration|容器|请求的容器 父元素：无

|

## 细节分析 {#section_mkj_ghw_bz .section}

-   如果Bucket不存在，返回404 no content错误。错误码：NoSuchBucket。
-   只有Bucket的拥有者才能查看Bucket的静态网站托管状态，否则返回403 Forbidden错误，错误码：AccessDenied。
-   如果源Bucket未设置静态网站托管功能，OSS会返回404错误，错误码为：NoSuchWebsiteConfiguration。

## 示例 {#section_wld_hhw_bz .section}

**请求示例：**

```
    Get /?website HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com   
    Date: Thu, 13 Sep 2012 07:51:28 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=

```

**已设置LOG规则的返回示例：**

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu, 13 Sep 2012 07:51:28 GMT
Connection: keep-alive
Content-Length: 218  
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<WebsiteConfiguration xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
<IndexDocument>
<Suffix>index.html</Suffix>
    </IndexDocument>
    <ErrorDocument>
        <Key>error.html</Key>
    </ErrorDocument>
</WebsiteConfiguration>
```

**未设置LOG规则的返回示例**

```
HTTP/1.1 404 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu, 13 Sep 2012 07:56:46 GMT
Connection: keep-alive
Content-Length: 308  
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<Error xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Code>NoSuchWebsiteConfiguration</Code>
    <Message>The specified bucket does not have a website configuration.</Message>
    <BucketName>oss-example</BucketName>
    <RequestId>505191BEC4689A033D00236F</RequestId>
    <HostId>oss-example.oss-cn-hangzhou.aliyuncs.com</HostId>
</Error>
```

