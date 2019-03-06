# Set ACLs {#concept_64185_zh .concept}

OSS allows you to set access permissions for objects. This helps to easily control external access to your resources.

You can set four ACLs for an object:

-   default: The object inherits the ACL for the bucket it belongs to, that is, the ACL for the object is the same as that for the bucket where the object is stored.
-   public-read-write: Anonymous users are allowed to read/write the object.
-   public-read: Anonymous users are allowed to read the object.
-   private: Anonymous users are not allowed to access objects in the bucket. Signature is required for all accesses.

When an object is created, the default permission applies by default. You can use `putACL` to set the ACL for the object.

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function getACL () {
  try {
    let result = await client.getACL('my-object');
	console.log(result.acl); // default
	
	await client.putACL('my-object', 'public-read');
	let result = await client.getACL('my-object');
    console.log(result.acl); // public-read
  } catch (e) {
    console.log(e);
  }
}

getACL();

```

**Note:** 

1.  If an object is configured with an ACL policy \(not default\), the object ACL takes priority during permission authentication when it is accessed. The bucket ACL is ignored.
2.  If anonymous access is allowed \(public-read or public-read-write is configured for the object\), you can directly access the object using a browser.

    ```
     For example: http://bucket-name.oss-cn-hangzhou.aliyuncs.com/object.jpg.
    
    ```


For more information about ACL, see [Access Control](../../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#).

