# PutBucket {#reference_wdh_fj5_tdb .reference}

PutBucket接口用于创建存储空间（Bucket）。

**说明：** 

-   此接口不支持匿名访问。
-   一个用户在同一地域（Region）内最多可创建30个Bucket。
-   每个地域都有对应的访问域名（Endpoint），地域与访问域名的对应关系参见[访问域名和数据中心](../../../../../intl.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#) 。

## 请求语法 {#section_w34_mmr_bz .section}

```
PUT / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
x-oss-acl: Permission
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration>
    <StorageClass>Standard</StorageClass>
</CreateBucketConfiguration>
```

## 请求头 {#section_asb_zxq_3gb .section}

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|x-oss-acl|字符串|否| 指定Bucket访问权限。

 有效值：public-read-write、public-read、private

 **说明：** 如果创建的 Bucket 没有指定访问权限，则默认使用private权限。

 |

## 请求元素 {#section_mtf_lwq_3gb .section}

|名称|类型|是否必选|描述|
|--|--|:---|--|
|StorageClass|字符串| | 指定Bucket存储类型。

 有效值：Standard、IA、Archive

 |
|DataRedundancyType|字符串|否| 指定Bucket的数据容灾类型。

 有效值：

-   LRS（本地容灾类型，默认值）
-   ZRS（同城容灾类型）

 |

## 示例 {#section_axr_rmr_bz .section}

**请求示例**

```
PUT / HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2017 03:15:40 GMT
x-oss-acl: private
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:77Dvh5wQgIjWjwO/KyRt8dOPfo8=
<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration>
    <StorageClass>Standard</StorageClass>
</CreateBucketConfiguration>
```

**返回示例**

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2017 03:15:40 GMT
Location: /oss-example
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../intl.zh-CN/SDK 参考/Java/存储空间.md)
-   [Python](../../../../../intl.zh-CN/SDK 参考/Python/存储空间.md)
-   [PHP](../../../../../intl.zh-CN/SDK 参考/PHP/存储空间.md)
-   [Go](../../../../../intl.zh-CN/SDK 参考/Go/存储空间.md)
-   [C](../../../../../intl.zh-CN/SDK 参考/C/存储空间.md)
-   [.NET](../../../../../intl.zh-CN/SDK 参考/.NET/存储空间.md)
-   [Android](../../../../../intl.zh-CN/SDK 参考/Android/存储空间.md)
-   [iOS](../../../../../intl.zh-CN/SDK 参考/iOS/存储空间.md)
-   [Node.js](../../../../../intl.zh-CN/SDK 参考/Node.js/存储空间.md)
-   [Ruby](../../../../../intl.zh-CN/SDK 参考/Ruby/存储空间.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|InvalidBucketName|400|创建的Bucket不符合命名规范。|
|AccessDenied|403| -   发起PutBucket请求时没有传入用户验证信息。
-   没有操作权限。

 |
|TooManyBuckets|400|创建的Bucket数量超过上限。 同一用户在同一 Region内最多可创建30个Bucket。|

