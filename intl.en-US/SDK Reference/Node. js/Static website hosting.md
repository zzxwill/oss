# Static website hosting {#concept_32081_zh .concept}

You can set your bucket to the static website hosting mode. After the configuration takes effect, you can access this static website with the bucket domain and be redirected to a specified index page or error page.

As mentioned in [CNAME](reseller.en-US/SDK Reference/Node. js/CNAME.md#), OSS allows you to direct your domain name to the OSS service address. By this means, OSS bucket is accessed when you access your website. You must specify the names of the objects in the bucket corresponding to the homepage \(index page\) and error page of the website respectively.

For more information, see [Static Website Hosting](../../../../../reseller.en-US/Developer Guide/Static website hosting/Configure static website hosting.md#).

## Set website hosting { .section}

Use `putBucketWebsite` to set website hosting:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function putBucketWebsite () {
  try {
    let result = await client.putBucketWebsite('bucket-name', {
    index: 'index.html',
    error: 'error.html'
  });
   console.log(result);
  } catch (e) {
    console.log(e);
  }
}

putBucketWebsite();

```

## View website hosting { .section}

Use `getBucketWebsite` to view website hosting:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function getBucketWebsite () {
  try {
    let result = await client.getBucketWebsite('bucket-name');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

getBucketWebsite();

```

## Clear website hosting { .section}

Use `deleteBucketWebsite` to clear website hosting:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function deleteBucketWebsite() {
  try {
    let result = await client.deleteBucketWebsite('bucket-name');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

deleteBucketWebsite();

```

