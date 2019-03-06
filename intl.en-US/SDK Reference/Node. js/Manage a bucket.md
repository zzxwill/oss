# Manage a bucket {#concept_32071_zh .concept}

A bucket is a namespace on OSS and a management entity for billing, access control, logging, and other advanced features.

## View all buckets {#section_l4m_ppk_lfb .section}

You can use the `listBuckets` interface to list all the buckets belonging to the current user. You can specify the `prefix` parameter to list all buckets whose names contain the specified prefix.

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>'
});

async function listBuckets() {
  try {
    const result = await client.listBuckets();
    console.log(result);
    const result2 = await client.listBuckets({
      prefix: 'prefix',
    });
    console.log(result);
  } catch (err) {
    console.log(err);
  }
}

listBuckets();

```

## Create a bucket { .section}

You can create a bucket using the `putBucket` interface. You require to specify the bucket name:

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>'
});

// putBucket
async function putBucket() {
  try {
    const result = await client.putBucket('your bucket name');
    console.log(result);
  } catch (err) {
    console.log(err);
  }
}

putBucket();

```

**Note:** 

-   For bucket naming conventions, see [OSS Basic Concepts](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#).
-   This is because the name of a bucket is globally unique, make sure your bucket name does not match with the other users’.

## Delete a bucket { .section}

You can use the `deleteBucket` interface to delete a bucket. You require to specify the bucket name:

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>'
});

async function deleteBucket() {
  try {
    const result = await client.deleteBucket('your bucket name');
    console.log(result);
  } catch (err) {
    console.log(err);
  }
}

deleteBucket();

```

**Note:** 

-   If the bucket to be deleted has objects in it, you must delete the objects first.
-   If the bucket to be deleted has unfinished upload requests, you must cancel the requests first using the `listUploads` and `abortMultipartUpload` interfaces.

## Bucket access permissions { .section}

You can set the ACL policy of a bucket to allow or prohibit anonymous reads/writes to the bucket. For more information on ACL, see [ACL](../../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#).

-   Obtain the ACL of a bucket

    You can use `getBucketACL` to view the ACL policy of a bucket:

    ```language-js
    let OSS = require('ali-oss');
    
    let client = new OSS({
      region: '<Your region>',
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>'
    });
    
    async function getBucketACL() {
      try {
        const result = await client.getBucketACL('luozhang002');
        console.log(result);
      } catch (err) {
        console.log(err);
      }
    }
    
    getBucketACL();
    
    ```

-   Set the ACL policy of a bucket

    You can use `putBucketACL` to configure the ACL policy of a bucket:

    ```language-js
    let OSS = require('ali-oss');
    
    let client = new OSS({
      region: '<Your region>',
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>'
    });
    
    async function putBucketACL() {
      try {
        const result = await client.putBucketACL('bucket name', 'public-read');
        console.log(result);
      } catch (err) {
        console.log(err);
      }
    }
    
    putBucketACL();
    
    ```


