# GetBucketLocation {#reference_e11_qtv_tdb .reference}

Views the location information about the data center \(region\) to which a bucket belongs. Only the owner of a bucket can view the region of the bucket.

## Request syntax {#section_tyd_bfw_bz .section}

```
GET /? Location HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_l4c_cfw_bz .section}

|Element|Type|Description|
|-------|----|-----------|
|Locationconstraint|String|Indicates the region where a bucket is located.Valid values:oss-cn-hangzhou, oss-cn-qingdao, oss-cn-beijing, oss-cn-hongkong, oss-cn-shenzhen, oss-cn-shanghai, oss-us-west-1, oss-us-east-1, and oss-ap-southeast-1

|

**Note:** For more information about the regions and the locations where the Alibaba Cloud data centers are located, see [Regions and endpoints](../../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).

## Examples {#section_f32_3fw_bz .section}

Request example:

```
Get /? location HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 04 May 2012 05:31:04 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=

```

Response example:

```
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 15 Mar 2013 05:31:04 GMT
Connection: keep-alive
Content-Length: 90 
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<LocationConstraint xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>oss-cn-hangzhou</LocationConstraint >
```

## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../../intl.en-US/SDK Reference/Java/Manage a bucket.md)
-   [PHP](../../../../../intl.en-US/SDK Reference/PHP/Bucket.md)
-   [Go](../../../../../intl.en-US/SDK Reference/Go/Bucket.md)
-   [C](../../../../../intl.en-US/SDK Reference/C/Bucket.md)

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|AccessDenied|403|You do not have the permission to view the region of a bucket. Only the owner of a bucket can view the region of the bucket.|

