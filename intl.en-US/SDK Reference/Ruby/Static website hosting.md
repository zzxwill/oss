# Static website hosting {#concept_32126_zh .concept}

You can set your bucket to the static website hosting mode. After the configuration takes effect, you can access this static website with the bucket domain and be redirected to a specified index page or error page.

In [CNAME](reseller.en-US/SDK Reference/Node. js/CNAME.md#), we mentioned that the OSS allows you to direct your domain name to the OSS service address. In this way, the OSS bucket is accessed when you access your website. You must specify the names of the objects in the bucket corresponding to the homepage \(index\) and error page \(error\) of the website respectively.

For more information, see [Static website hosting](../../../../../reseller.en-US/Developer Guide/Static website hosting/Configure static website hosting.md#).

## Configure website hosting {#section_dkd_bgz_lfb .section}

Use `Bucket#website=` to configure hosted pages:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
bucket.website = BucketWebsite.new(index: 'index.html', error: 'error.html')

```

## View website hosting { .section}

Use `Bucket#website` to display hosted pages:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
web = bucket.website
puts web.to_s

```

## Clear website hosting { .section}

Use `Bucket#website=` to clear hosted pages:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
bucket.website = BucketWebsite.new(enable: false)

```

