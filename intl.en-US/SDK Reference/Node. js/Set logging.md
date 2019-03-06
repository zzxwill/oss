# Set logging {#concept_32080_zh .concept}

OSS allows you to configure bucket access logging to store the bucket access logs in a specified bucket for future analysis. The logs are stored as objects in the specified bucket.

The object name format is as follows:

```
<TargetPrefix><SourceBucket>-YYYY-mm-DD-HH-MM-SS-UniqueString

```

The `TargetPrefix` needs to be specified by the user during configuration. For more information about access logging, see [Bucket Access Logging](../../../../../reseller.en-US/Developer Guide/Manage logs/Set access logging.md#).

## Enable bucket logging {#section_cqx_xvk_lfb .section}

Use `putBucketLogging` to enable bucket logging:

```
let OSS = require('ali-oss')
let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});
async function putBucketLogging () {
  try {
     let result = await client.putBucketLogging('bucket-name', 'logs/');
     console.log(result)
  } catch (e) {
    console.log(e)
  }
}
putBucketLogging();
```

## View bucket logging settings { .section}

Use `getBucketLogging` to view bucket logging settings:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function getBucketLogging() {
  try {
     let result = await client.getBucketLogging('bucket-name');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
})

getBucketLogging();

```

## Disable bucket logging { .section}

Use `deleteBucketLogging` to disable bucket logging:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function deleteBucketLogging () {
  try {
    let result = await client.deleteBucketLogging('bucket-name');
    console.log(result);
  } catch (e) {
    console.log(e);
  }

deleteBucketLogging();

```

