# Initialization {#concept_d5g_1y1_dhb .concept}

This topic describes how to initialize the Node.js SDK.

Create an object named `app.js` and write the following content:

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>'
});

```

The `region` field specifies the region that is set when you apply for OSS, such as `oss-cn-hangzhou`. For the complete list of regions, see [Endpoints and regions](../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).

**Note:** If the desired endpoint is not included in the preceding list, you can use the following parameters to specify the endpoint:

-   internal: You must set the `region` field if the internal parameter is used. If you set `internal` to `true`, you can access the intranet nodes.
-   secure: You must set the `region` field if the secure parameter is used. If you set `secure` to `true`, use HTTPS for access.
-   endpoint: If you specify the `endpoint` field, such as `http://oss-cn-hangzhou.aliyuncs.com`, you do not need to set the `region` field. The `endpoint` field value can be a domain that uses HTTPS or an IP address.
-   cname: You must set the `endpoint` field if the cname parameter is used. If you set `cname` to `true`, the `endpoint` field value is used as the custom domain that is bound to a bucket.
-   bucket: If the `bucket` field value is not specified, you must first call `useBucket` to perform operations on the object that belongs to the bucket. You need to call this API only once.
-   timeout: specifies the timeout period to access OSS APIs. The default value is 60 seconds.

