# CNAME {#concept_32121_zh .concept}

This topic describes how to bind a custom domain name.

OSS supports binding custom domain names to OSS to seamlessly migrate data storage to the OSS. For example, your domain name is my-domain.com, and all your previous image resources are in a format similar to `http://img.my-domain.com/x.jpg`. After you migrate the image storage to OSS, you can still access the images with the original address by binding your custom domain name to OSS:

1.  Activate OSS and create a bucket
2.  Bind img.my-domain.com to the created bucket.
3.  Upload images to the created bucket in OSS.
4.  Modify the DNS configuration, add a CNAME record, and direct img.my-domain.com to an endpoint of OSS \(for example, my-bucket.oss-cn-hangzhou.aliyuncs.com\)

Follow the preceding steps to use the original address `http://img.my-domain.com/xx.jpg` to access images on OSS. For more information, see [Bind custom domain names \(CNAME\)](../../../../../reseller.en-US/Developer Guide/Buckets/Attach a custom domain name.md#).

SDK supports the use of a CNAME as the endpoint. To do this, set the `:cname` parameter to true. For example:

```language-ruby

require 'aliyun/oss'

include Aliyun::OSS

client = Client.new(
  endpoint: 'ENDPOINT',
  access_key_id: 'ACCESS_KEY_ID',
  access_key_secret: 'ACCESS_KEY_SECRET',
  cname: true)

bucket = client.get_bucket('my-bucket')


```

**Note:** The list\_buckets interface is unavailable when CNAME is used, because the custom domain name is bound already to a specific bucket.

