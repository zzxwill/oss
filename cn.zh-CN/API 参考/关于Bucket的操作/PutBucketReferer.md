# PutBucketReferer {#reference_prc_ys5_tdb .reference}

PutBucketReferer接口用于设置Bucket的Referer访问白名单以及允许Referer字段是否为空。

## 请求语法 {#section_yfn_hcw_bz .section}

```
PUT /?referer HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue

<?xml version="1.0" encoding="UTF-8"?>
<RefererConfiguration>
<AllowEmptyReferer>true</AllowEmptyReferer >
    <RefererList>
        <Referer> http://www.aliyun.com</Referer>
        <Referer> https://www.aliyun.com</Referer>
        <Referer> http://www.*.com</Referer>
        <Referer> https://www.?.aliyuncs.com</Referer>
    </RefererList>
</RefererConfiguration>
```

## 请求元素（Request Elements） {#section_ujq_jcw_bz .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|RefererConfiguration|容器|是|保存Referer配置内容的容器。子节点：AllowEmptyReferer节点、RefererList节点

父节点：无

|
|AllowEmptyReferer|枚举字符串|是|指定是否允许Referer字段为空的请求访问。取值：

-   true \(默认值）
-   false

父节点：RefererConfiguration

 |
|RefererList|容器|是|保存Referer访问白名单的容器。父节点：RefererConfiguration

子节点：Referer

 |
|Referer|字符串|否|指定一条Referer访问白名单。父节点：RefererList

|

## 细节分析 {#section_fcy_lcw_bz .section}

-   只有Bucket的拥有者才能发起PutBucketReferer请求，否则返回403 Forbidden消息。错误码：AccessDenied。
-   AllowEmptyReferer中指定的配置将替换之前的AllowEmptyReferer配置，该字段为必填项，系统中默认的AllowEmptyReferer配置为true。
-   PutBucketReferer操作将用RefererList中的白名单列表覆盖之前配置的白名单列表。当用户上传的RefererList为空时（不包含Referer请求元素），此操作会覆盖已配置的白名单列表，即删除之前配置的RefererList。
-   如果用户上传了Content-MD5请求头，OSS会计算body的Content-MD5并检查一致性。如果不一致，将返回InvalidDigest错误码。

## 示例 {#section_ovw_mcw_bz .section}

不包含Referer的请求示例：

```
PUT /?referer HTTP/1.1
Host: BucketName.oss.example.com
Content-Length: 247
Date: Fri, 04 May 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=

<?xml version="1.0" encoding="UTF-8"?>
<RefererConfiguration>
<AllowEmptyReferer>true</AllowEmptyReferer >
< RefererList />
</RefererConfiguration>

```

包含Referer的请求示例：

```
PUT /?referer HTTP/1.1
Host: BucketName.oss.example.com
Content-Length: 247
Date: Fri, 04 May 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=

<?xml version="1.0" encoding="UTF-8"?>
<RefererConfiguration>
<AllowEmptyReferer>true</AllowEmptyReferer >
< RefererList>
<Referer> http://www.aliyun.com</Referer>
<Referer> https://www.aliyun.com</Referer>
<Referer> http://www.*.com</Referer>
<Referer> https://www.?.aliyuncs.com</Referer>
</ RefererList>
</RefererConfiguration>

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

