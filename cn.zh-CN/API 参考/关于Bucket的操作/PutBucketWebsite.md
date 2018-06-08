# PutBucketWebsite {#reference_hwb_yr5_tdb .reference}

PutBucketWebsite接口用于将一个bucket设置成静态网站托管模式。

## 请求语法 {#section_o3z_sn2_xdb .section}

```
PUT /?website HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue

<?xml version="1.0" encoding="UTF-8"?>
<WebsiteConfiguration>
    <IndexDocument>
        <Suffix>index.html</Suffix>
    </IndexDocument>
    <ErrorDocument>
        <Key>errorDocument.html</Key>
    </ErrorDocument>
</WebsiteConfiguration>
```

## 请求元素（Request Elements） {#section_d4t_vn2_xdb .section}

|名称|类型|描述|是否必须|
|:-|:-|:-|:---|
|ErrorDocument|容器|子元素Key的父元素父元素: WebsiteConfiguration

|否|
|IndexDocument|容器|子元素Suffix的父元素. 父元素: WebsiteConfiguration

|是|
|Key|字符串|返回404错误时使用的文件名 父元素: WebsiteConfiguration.ErrorDocument

有条件：当ErrorDocument设置时，必需

|有条件|
|Suffix|字符串|返回目录URL时添加的索引文件名，不要为空，也不要包含"/"。例如索引文件设置为index.html，则访问:oss-cn-hangzhou.aliyuncs.com/mybucket/mydir/这样请求的时候默认都相当于访问oss-cn-hangzhou.aliyuncs.com/mybucket/index.html 父元素: WebsiteConfiguration.IndexDocument

|是|
|WebsiteConfiguration|容器|请求的容器 父元素: 无

|是|

## 细节分析 {#section_hms_f42_xdb .section}

-   所谓静态网站是指所有的网页都由静态内容构成，包括客户端执行的脚本，例如JavaScript；OSS不支持涉及到需要服务器端处理的内容，例如PHP，JSP，APS.NET等。
-   如果你想使用自己的域名来访问基于bucket的静态网站，可以通过域名CNAME来实现。具体配置方法请参见[绑定自定义域名](../cn.zh-CN//绑定自定义域名.md#)。
-   用户将一个bucket设置成静态网站托管模式时，必须指定索引页面，错误页面则是可选的。
-   用户将一个bucket设置成静态网站托管模式时，指定的索引页面和错误页面是该bucket内的一个object。
-   在将一个bucket设置成静态网站托管模式后，对静态网站根域名的匿名访问，OSS将返回索引页面；对静态网站根域名的签名访问，OSS将返回Get Bucket结果。
-   如果用户上传了Content-MD5请求头，OSS会计算body的Content-MD5并检查一致性，如果不一致，将返回InvalidDigest错误码。

## 示例 {#section_c33_k42_xdb .section}

**请求示例：**

```
PUT /?website HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length: 209
Date: Fri, 04 May 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=

<?xml version="1.0" encoding="UTF-8"?>
<WebsiteConfiguration>
<IndexDocument>
<Suffix>index.html</Suffix>
</IndexDocument>
<ErrorDocument>
<Key>error.html</Key>
</ErrorDocument>
</WebsiteConfiguration>
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

