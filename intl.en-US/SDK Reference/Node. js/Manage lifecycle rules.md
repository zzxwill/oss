# Manage lifecycle rules {#concept_32079_zh .concept}

OSS allows you to set lifecycle rules for buckets to automatically remove expired objects and save the storage space.

You can configure lifecycle rules for OSS buckets or objects to automatically delete expired objects and parts or change the storage class of expired objects to IA or Archive, saving storage costs. A lifecycle rule includes the following elements:

-   Rule ID: Identifies a lifecycle rule. A rule ID is unique in a bucket.
-   Strategy: You can set one of the following two application strategies for a rule. The strategy for all rules set for a bucket must be the same.
    -   The rule applies to objects with specified prefixes. You can create multiple rules with this strategy. Prefixes specified in different rules must be different.
    -   The rule applies to the entire bucket. You can create only one rule with this strategy.
-   Expiration period:
    -   Specifies the expiration period \(N days\) of a lifecycle rule. An object expires N days after it is modified for the last time.
    -   Specifies an expiration time. Objects modified before the time expire.
-   Status: Indicates whether the rule is enabled or disabled.

For more information, see [Lifecycle rules](../../../../reseller.en-US/Developer Guide/Manage files/Manage lifecycle rules.md#).

## Configure lifecycle rules {#section_rl5_kvk_lfb .section}

You can use `putBucketLifecycle` to configure lifecycle rules.

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function putBucketLifecycle () {
  try {
    let result = yield client.putBucketLifecycle('bucket-name', [
    {
      id: 'rule1',
      status: 'Enabled',
      prefix: 'foo/',
      days: 3
    },
    {
      id: 'rule2',
      status: 'Disabled',
      date: '2022-10-11T00:00:00.000Z'
    }
  ]);
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

putBucketLifecycle();
			
```

## View lifecycle rules { .section}

You can use `getBucketLifecycle` to view lifecycle rules:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function getBucketLifecycle () {
  try {
    let result = await client.getBucketLifecycle('bucket-name');
  console.log(result);
  } catch (e) {
    console.log(e);
  }
}

getBucketLifecycle();
			
```

## Clear lifecycle rules { .section}

You can use `deleteBucketLifecycle` to set a null Rule array to clear the lifecycle rules:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function deleteBucketLifecycle () {
  try {
    let result = await client.deleteBucketLifecycle('bucket-name');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

deleteBucketLifecycle();
			
```

