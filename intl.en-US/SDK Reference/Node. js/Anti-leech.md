# Anti-leech {#concept_32082_zh .concept}

This topic describes how to use anti-leech.

To prevent your data on OSS from being leeched, OSS supports anti-leeching through the referer field settings in the HTTP header, including the following parameters:

-   Referer whitelist: Used to allow access only for specified domains to OSS data.
-   Empty referer: Determines whether the referer can be empty. If it is not allowed, only requests with the referer filed in their HTTP or HTTPS headers can access OSS data.

For more information about anti-leach, see [Anti-leech settings](../../../../reseller.en-US/Developer Guide/Buckets/Anti-leech settings.md#).

## Configure a referer whitelist {#section_tt5_5wk_lfb .section}

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

