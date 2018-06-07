# GetBucketReferer {#reference_bs5_rpw_tdb .reference}

GetBucketReferer操作用于查看bucket的Referer相关配置。

## 请求语法 {#section_j24_4hw_bz .section}

```
GET /?referer HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素\(Response Elements\) {#section_jzk_phw_bz .section}

|名称|类型|描述|
|--|--|--|
|RefererConfiguration|容器|保存Referer配置内容的容器。子节点：AllowEmptyReferer节点、RefererList节点

父节点：无

|
|AllowEmptyReferer|枚举字符串|指定是否允许referer字段为空的请求访问。取值：-   true （默认值）
-   false

父节点：RefererConfiguration

|
|RefererList|容器|保存referer访问白名单的容器。 父节点：RefererConfiguration

子节点：Referer

|
|Referer|字符串|指定一条referer访问白名单。 父节点：RefererList

|

## 细节分析 {#section_gvp_rhw_bz .section}

-   如果Bucket不存在，返回404错误。错误码：NoSuchBucket。
-   只有Bucket的拥有者才能查看Bucket的Referer配置信息，否则返回403 Forbidden错误，错误码：AccessDenied。
-   如果Bucket未进行Referer相关配置，OSS会返回默认的AllowEmptyReferer值和空的RefererList。

## 示例 {#section_hr3_shw_bz .section}

**请求示例：**

```
Get /?referer HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Thu, 13 Sep 2012 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=
```

**已设置Referer规则的返回示例：**

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu, 13 Sep 2012 07:51:28 GMT
Connection: keep-alive
Content-Length: 218  
Server: AliyunOSS

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

**未设置Referer规则的返回示例：**

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu, 13 Sep 2012 07:56:46 GMT
Connection: keep-alive
Content-Length: 308  
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<RefererConfiguration>
<AllowEmptyReferer>true</AllowEmptyReferer >
< RefererList />
</RefererConfiguration>
```

