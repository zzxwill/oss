# Getbucketlocation {#reference_e11_qtv_tdb .reference}

GetBucketLocation is used to view the location information about the data center to which a bucket belongs.

## Request syntax {#section_tyd_bfw_bz .section}

```
GET /? Location HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_l4c_cfw_bz .section}

|Name|Type|Description|
|----|----|-----------|
|Locationconstraint|String|Region where a bucket is located.Values:oss-cn-hangzhou、oss-cn-qingdao、oss-cn-beijing、oss-cn-hongkong、oss-cn-shenzhen、oss-cn-shanghai

|

## Detail analysis {#section_f5l_hfw_bz .section}

-   Only the owner of a bucket can view the location information of the bucket. If other users attempt to access the location information, the error 403 Forbidden with the error code: AccessDenied is returned.
-   LocationConstraint has the following valid values: oss-cn-hangzhou，oss-cn-qingdao，oss-cn-beijing，oss-cn-hongkong，oss-cn-shenzhen，oss-cn-shanghai，oss-us-west-1，oss-us-east-1，and oss-ap-southeast-1,  which separately indicate the Hangzhou data center, Qingdao data center, Beijing data center, Hong Kong data center, Shenzhen data center, Shanghai data center, US Silicon Valley data center, US, Virginia data center, and Asia-Pacific \(Singapore\) data center.

## Examples {#section_f32_3fw_bz .section}

**Request example:**

```
Get /? location HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 04 May 2012 05:31:04 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3naA=

```

**Response example with logging rules configured:**

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

