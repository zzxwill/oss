# GetBucketLocation {#reference_e11_qtv_tdb .reference}

GetBucketLocation接口用于查看存储空间（Bucket）的位置信息。只有Bucket的拥有者才能查看Bucket的位置信息。

## 请求语法 {#section_tyd_bfw_bz .section}

```
GET /?location HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素 {#section_l4c_cfw_bz .section}

|名称|类型|描述|
|:-|:-|:-|
|LocationConstraint|字符串| Bucket所在的地域

 有效值：oss-cn-hangzhou、oss-cn-qingdao、oss-cn-beijing、oss-cn-hongkong、oss-cn-shenzhen、oss-cn-shanghai、oss-us-west-1、oss-us-east-1、oss-ap-southeast-1

 |

**说明：** Bucket所在地域与数据中心的对应关系参见[访问域名与数据中心](../../../../../intl.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#)。

## 示例 {#section_f32_3fw_bz .section}

**请求示例**

```
Get /?location HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 04 May 2012 05:31:04 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=

```

**返回示例**

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 15 Mar 2013 05:31:04 GMT
Connection: keep-alive
Content-Length: 90 
Server: AliyunOSS

<?xml version="1.0" encoding="UTF-8"?>
<LocationConstraint xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>oss-cn-hangzhou</LocationConstraint >
```

## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../intl.zh-CN/SDK 参考/Java/存储空间.md)
-   [PHP](../../../../../intl.zh-CN/SDK 参考/PHP/存储空间.md)
-   [Go](../../../../../intl.zh-CN/SDK 参考/Go/存储空间.md)
-   [C](../../../../../intl.zh-CN/SDK 参考/C/存储空间.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|AccessDenied|403|没有查看Bucket位置信息的权限。只有Bucket的拥有者才能查看Bucket的位置信息。|

