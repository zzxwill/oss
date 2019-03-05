# GetBucketReferer {#reference_bs5_rpw_tdb .reference}

GetBucketReferer接口用于查看存储空间（Bucket）的防盗链（Referer）相关配置。

## 请求语法 {#section_j24_4hw_bz .section}

```
GET /?referer HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素 {#section_jzk_phw_bz .section}

|名称|类型|描述|
|:-|:-|:-|
|RefererConfiguration|容器| 保存Referer配置内容的容器。

 父节点：无

 子节点：AllowEmptyReferer，RefererList

 |
|AllowEmptyReferer|枚举字符串| 指定是否允许Referer字段为空的请求访问。

 取值：true，false

 父节点：RefererConfiguration

 |
|RefererList|容器| 保存Referer访问白名单的容器。

 父节点：RefererConfiguration

 子节点：Referer

 |
|Referer|字符串| 指定一条Referer的访问白名单。

 父节点：RefererList

 |

## 示例 {#section_hr3_shw_bz .section}

**请求示例**

```
Get /?referer HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Thu, 13 Sep 2012 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39zXw=
```

**返回示例（已设置Referer规则）**

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

**返回示例（未设置Referer规则）**

**说明：** 如果Bucket未进行Referer相关配置，OSS会返回默认的AllowEmptyReferer值和空的RefererList。

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

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../intl.zh-CN/SDK 参考/Java/防盗链.md)
-   [Python](../../../../../intl.zh-CN/SDK 参考/PHP/防盗链.md)
-   [PHP](../../../../../intl.zh-CN/SDK 参考/PHP/防盗链.md)
-   [Go](../../../../../intl.zh-CN/SDK 参考/Go/防盗链.md)
-   [C](../../../../../intl.zh-CN/SDK 参考/C/防盗链.md)
-   [.NET](../../../../../intl.zh-CN/SDK 参考/.NET/防盗链.md)
-   [Node.js](../../../../../intl.zh-CN/SDK 参考/Node.js/防盗链.md)
-   [Ruby](../../../../../intl.zh-CN/SDK 参考/Ruby/防盗链.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|AccessDenied|403|没有查看Bucket的Referer配置信息的权限。只有Bucket的拥有者才能查看Bucket的Referer配置信息。|

