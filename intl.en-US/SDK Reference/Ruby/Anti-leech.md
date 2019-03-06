# Anti-leech {#concept_32127_zh .concept}

OSS is a Pay-As-You-Go service. To prevent users’ data on the OSS from being leeched, OSS supports anti-leech based on the referer field in the HTTP header.

为了防止您在OSS上的数据被其他人盗链而产生额外费用，您可以设置防盗链功能，包括以下参数：

-   Referer白名单。仅允许指定的域名访问OSS资源。
-   是否允许空Referer。如果不允许空Referer，则只有HTTP或HTTPS header中包含Referer字段的请求才能访问OSS资源。

For more information about the OSS anti-leech features, see [Anti-leech settings](../../../../../reseller.en-US/Developer Guide/Buckets/Anti-leech settings.md#).

## Configure a Referer whitelist { .section}

The following code uses `Bucket#referer=` to configure a referer whitelist:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
bucket.referer = BucketReferer.new(
  allow_empty: true, whitelist: ['my-domain.com', '*.example.com'])

```

## View a referer whitelist { .section}

The following code uses `Bucket#referer` to set a referer whitelist:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
ref = bucket.referer
puts ref.to_s

```

## Clear a referer whitelist { .section}

The following code uses `Bucket#referer=` to clear a referer whitelist:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
bucket.referer = BucketReferer.new(allow_empty: true, whitelist: [])

```

