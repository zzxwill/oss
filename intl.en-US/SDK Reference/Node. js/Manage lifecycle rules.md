# Manage lifecycle rules {#concept_32079_zh .concept}

OSS allows you to set lifecycle rules for buckets to automatically remove expired objects and save the storage space.

OSS支持设置生命周期（Lifecycle）规则，自动删除过期的文件和碎片，或将到期的文件转储为低频或归档存储类型，从而节省存储费用。每条规则包含：

-   规则ID。用于标识一条规则，同一存储空间内规则ID不能重复。
-   策略。有以下两种设置方式。同一存储空间内仅支持一种设置方式。
    -   按前缀匹配。此种方式允许创建多条规则，前缀不能重复。
    -   配置到整个存储空间。此种方式只能创建一条规则。
-   过期时间。有两种指定方式：
    -   指定一个过期天数N，文件会在其最近更新时间点的N天后过期。
    -   指定一个过期时间点，最近更新时间在该时间点之前的文件全部过期。
-   是否生效。

For more information, see [Lifecycle rules](../../../../../reseller.en-US/Developer Guide/Manage files/Manage object lifecycle.md#).

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

