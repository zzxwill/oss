# Set ACLs {#concept_32078_zh .concept}

This topic describes how to set ACLs.

OSS allows you to set ACLs for buckets and objects respectively. This helps to easily control external access to your resources. You can set three ACLs for a bucket:

-   public-read-write: Anonymous users are allowed to create/retrieve/delete objects in the bucket.
-   public-read: Anonymous users are allowed to retrieve objects in the bucket.
-   private: Anonymous users are not allowed to access objects in the bucket. Signature is required for all accesses.

When a bucket is created, the private ACL applies by default. You can use `putBucketACL` to set bucket ACLs and use `getBucketACL` to get the bucket ACL.

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function bucketACL () {
  try {
    let result = await client.getBucketACL('bucket-name');
    console.log(result);
	
	let result = await client.putBucketACL('bucket-name', 'acl');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

bucketACL();

```

You can set four ACLs for an object.

-   default: The object inherits the ACL for the bucket it belongs to, that is, the ACL for the object is the same as that for the bucket where the object is stored.
-   public-read-write: Anonymous users are allowed to read/write the object.
-   public-read: Anonymous users are allowed to read the object.
-   private: Anonymous users are not allowed to access objects in the bucket. Signature is required for all accesses.

When an object is created, the default ACL applies by default. You can use `putACL` to set the object ACL.

```
let OSS = require('ali-oss')
let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});
async function bucketACL () {
  try {
     let result = await client.getACL('my-object');
     console.log(result.acl); // default
     await client.putACL('my-object', 'public-read');
     result = yield client.getACL('my-object');
     console.log(result.acl); // public-read
  } catch (e) {
    console.log(e);
  }
}
```

**Note:** 

1.  If an object is configured with an ACL policy \(not default\), the object ACL takes priority during permission authentication when it is accessed. The bucket ACL is ignored.
2.  If anonymous access is allowed \(public-read or public-read-write is configured for the object\), you can directly access the object using a browser.

    ```
     For example: http://bucket-name.oss-cn-hangzhou.aliyuncs.com/object.jpg.
    
    ```


For more information about ACL, see [Access Control](../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#).

