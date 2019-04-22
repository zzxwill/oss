# Anti-leech {#concept_32127_zh .concept}

OSS is a Pay-As-You-Go service. To prevent users’ data on the OSS from being leeched, OSS supports anti-leech based on the referer field in the HTTP header.

To prevent your data on OSS from being leeched, OSS supports anti-leeching through the referer field settings in the HTTP header, including the following parameters:

-   Referer whitelist: Used to allow access only for specified domains to OSS data.
-   Empty referer: Determines whether the referer can be empty. If it is not allowed, only requests with the referer filed in their HTTP or HTTPS headers can access OSS data.

For more information about the OSS anti-leech features, see [Anti-leech settings](../../../../reseller.en-US/Developer Guide/Buckets/Anti-leech settings.md#).

## Configure a referer whitelist { .section}

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

