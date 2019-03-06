# CNAME {#concept_32076_zh .concept}

This topic describes how to bind a custom domain name.

OSS supports binding custom domain names to OSS, so that you can seamlessly migrate data storage to OSS. For example, if your domain name is "my-domain.com" and all your images were previously located at `http://img.my-domain.com/x.jpg`, after you've moved your storage to OSS and linked your custom domain name, the images can still be accessed at the original address.

-   Activate OSS and create a bucket.
-   Modify the DNS configuration, add a CNAME record, and direct img.my-domain.com to an endpoint of OSS \(for example, my-bucket.oss-cn-hangzhou.aliyuncs.com\).
-   Upload images to the created bucket in OSS.

By following the preceding steps, you can use the original address http://img.my-domain.com/xx.jpg to access the images on OSS. For more information on how to bind custom domain names, see [Bind Custom Domain Names](../../../../../reseller.en-US/Developer Guide/Buckets/Attach a custom domain name.md#).

The SDK supports using a CNAME as the endpoint. Therefore, set the `cname` parameter to true.

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  endpoint: '<Your endpoint>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  cname: true
});

client.useBucket('my-bucket')

```

**Note:** The list\_buckets interface is unavailable when a CNAME is used as the endpoint, because the custom domain name has been bound to a specific bucket.

