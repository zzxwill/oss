# Anti-leech {#concept_32082_zh .concept}

This topic describes how to use anti-leech.

为了防止您在OSS上的数据被其他人盗链而产生额外费用，您可以设置防盗链功能，包括以下参数：

-   Referer白名单。仅允许指定的域名访问OSS资源。
-   是否允许空Referer。如果不允许空Referer，则只有HTTP或HTTPS header中包含Referer字段的请求才能访问OSS资源。

For more information about anti-leach, see [Anti-leech settings](../../../../../reseller.en-US/Developer Guide/Buckets/Anti-leech settings.md#).

## Configure a Referer whitelist {#section_tt5_5wk_lfb .section}

Use `putBucketReferer` to set a referer whitelist:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function putBucketReferer () {
  try {
  let result = await client.putBucketReferer('bucket-name', true, [
    'my-domain.com',
    '*.example.com'
  ]);
  console.log(result);
  } catch (e) {
    console.log(e);
  }
 }
 
putBucketReferer();

```

## View a referer whitelist { .section}

Use `getBucketReferer` to view a referer whitelist:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function getBucketReferer () {
  try {
    let result = await client.getBucketReferer('bucket-name');
	console.log(result);
  } catch (e) {
    console.log(e);
  }
}

getBucketReferer();

```

## Clear a referer whitelist { .section}

Use `deleteBucketReferer` to clear a referer whitelist:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function deleteBucketReferer () {
  try {
    let result = await client.deleteBucketReferer('bucket-name');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

deleteBucketReferer();

```

