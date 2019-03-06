# Manage objects {#concept_32074_zh .concept}

A bucket may contain a lot of objects. The SDK provides a series of APIs for you to conveniently manage the objects.

## View all objects {#section_vk4_ksk_lfb .section}

The following code uses `list` to list all objects in the current bucket. Main parameters are described as follows:

-   prefix: Only list objects with specific prefixes.
-   marker: Only list objects with names after the specified marker.
-   delimiter: Obtain the common prefix of a group of objects.
-   max-keys: Specify the maximum number of objects to be returned.

```
let OSS = require('ali-oss');
let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});
async function list () {
  {
      // If no parameter is specified, a maximum of 1,000 objects can be returned by default.
    let result = await client.list();
    console.log   result);
  // Continue to list objects based on nextMarker.
    if (result.isTruncated) {
      let result = await client.list({
        marker: result.nextMarker
      });
    }
  // List all objects that are prefixed with "my-".
  let result = await client.list({
     prefix: 'my-'
  });
  console.log(result);
  // List all objects whose names are prefixed with "my-" and after the marker of "my-object".
  let result = await client.list({
     prefix: 'my-',
     marker: 'my-object'
  });
  console.log(result);
  } catch (e) {
    console.log(e);
  }
}
list();
```

## Simulate the directory structure { .section}

OSS is an object-based storage service. It has no directory. OSS is an object-based storage service. It has no directory. All objects stored in a bucket are uniquely identified bythe key of the object, and they are not hierarchical. Such a structure allows users to efficiently store objects in OSS,but some users may want to manage objects in a conventional way by putting different types of objectsunder different “directories”. OSS provides a “Common Prefix” feature to convenientlycreate a stimulated directory structure. For more information about the common prefix, see [List Objects](../../../../../reseller.en-US/Developer Guide/Manage files/View the object list.md#).

Assume that the bucket already contains the following objects:

```
foo/x
foo/y
foo/bar/a
foo/bar/b
foo/hello/C/1
foo/hello/C/2
...
foo/hello/C/9999

```

Next, we implement a function called listDir to list all the objects and subdirectories under the specified directory:

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function listDir(dir)
  let result = await client.list({
    prefix: dir,
    delimiter: '/'
  });

  result.prefixes.forEach(function (subDir) {
    console.log('SubDir: %s', subDir);
  });
  result.objects.forEach(function (obj) {
    console.log(Object: %s', obj.name);
  });
end

```

The results are as follows:

```
> await listDir('foo/')
=> SubDir: foo/bar/
   SubDir: foo/hello/
   Object: foo/x
   Object: foo/y

> await listDir('foo/bar/')
=> Object: foo/bar/a
   Object: foo/bar/b

> await listDir('foo/hello/C/')
=> Object: foo/hello/C/1
   Object: foo/hello/C/2
   ...
   Object: foo/hello/C/9999

```

## Object Meta { .section}

When uploading an object to OSS, you can specify certain property information about the object in addition to the content. The property information is called Object Meta, which is stored together with the object during uploading.

Object Meta is stored in HTTP headers during object uploads or downloads. Therefore, Object Meta cannot contain complex characters in accordance with HTTP.

**Note:** Object Meta must only contain simple printable ASCII characters and cannot contain line breaks. All meta information cannot exceed 8 KB.

You can specify the object meta information by specifying the meta parameter when using the `put` and ```multipartUpload`:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function put () {
  try {
    let result = await client.put('object-name', 'local-file', {
    meta: {
      year: 2016,
      people: 'mary'
    }
  });
  console.log(result);
  } catch (e) {
    console.log(e);
  }
}

put();

```

Update the object meta information through the putMeta interface:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function putMeta () {
  try {
    let result = await client.putMeta('object-name', {
    meta: {
      year: 2015,
      people: 'mary'
    }
  });
  console.log(result);
  } catch (e) {
    console.log(e);
  }
}

putMeta();

```

## Copy objects { .section}

Copy an object using `copy`. The copy may fall under the following two types of scenarios:

-   Copy objects within the same bucket
-   Copy object between two different bucket in the same region. The name of the original object must be in the /bucket/object format.

In addition, when an object is copied, there are two methods for processing the object metadata:

-   If no meta parameter is specified, the metadata remains the same as that of the source object, that is, copying the source object metadata.
-   If the meta parameter is specified, overwrite the source object information with the new metadata.

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function copy () {
  try {
     // Copying between two buckets
    let result = await client.copy('to', '/from-bucket/from');
    console.log(result);

    // Copy the meta information
    let result = await client.copy('to', 'from');
    console.log(result);

    // Overwrite the meta information
    let result = await client.copy('to', 'from', {
      meta: {
        year: 2015,
        people: 'mary'
      }
    });
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

```

## Delete an object { .section}

You can use `delete` to delete an object:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function delete () {
  try {
    let result = yield client.delete('object-name');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

delete();

```

## Delete objects in batch { .section}

The following code uses `deleteMulti` to delete objects in batch. You can set the quiet parameter to determine whether to return the deletion results:

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function deleteMulti () {
  try {
    let result = await client.deleteMulti(['obj-1', 'obj-2', 'obj-3']);
    console.log(result);
    let result = await client.deleteMulti(['obj-1', 'obj-2', 'obj-3'], {
    quiet: true
  });
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

deleteMulti();

```

