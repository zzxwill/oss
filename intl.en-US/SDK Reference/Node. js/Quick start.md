# Quick start {#concept_32070_zh .concept}

This topic describes how to use the OSS JavaScript SDK in a Node.js environment to access the OSS service, including viewing the bucket list, viewing the object list and uploading/downloading/deleting objects.

To facilitate changes, how to create a new `app.js` is explained in the next document. The demo code of the following features is included in this object.

## Install the SDK {#section_ujc_hnk_lfb .section}

First install `ali-oss` in the working directory:

```language-bash
npm install ali-oss

```

-   Use the synchronous mode

    SDK is developed on the basis of [ES6](https://github.com/lukehoban/es6features). Using Generator Function enables users to use the code in synchronous mode. This feature must be used in combination with `co`.

-   Use the asynchronous mode

    To support the callback usage, SDK provides asynchronous interfaces based on [Promise](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise). The usage is similar to the callback.[https://cnodejs.org/topic/570f8961400ca111729e8858](https://cnodejs.org/topic/570f8961400ca111729e8858)

    For example, the following section uses the synchronous mode:


## Initialize the client { .section}

Create an object `app.js` and write the following content:

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  AccessKeySecret: "<your-access-key-secret>",
});

```

Specifically, the `region` parameter is the region used when you applied for the OSS service, such as `oss-cn-hangzhou`. For complete region list, see [OSS Nodes](../../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).

**Note:** If the used endpoint is not in the preceding list, you can specify the endpoint through the following parameters:

-   internal: Use in combination with `region`. If `internal` is specified to `true`, visit the intranet node.
-   secure: Use in combination with `region`. If `secure` is specified to `true`, use the HTTPS for access.
-   endpoint: For example, `http://oss-cn-hangzhou.aliyuncs.com`. If `endpoint` is specified, `region` is ignored. You can specify HTTPS for `endpoint`, which can be in the IP address format.
-   cname: Use in combination with `endpoint`. If the `cname` is specified to `true`, the `endpoint` is regarded as the bounded user-defined domain of the user.
-   bucket: If the `bucket` is not specified, you must callthe `useBucket` interface first for object-related operations \(only one call is required\).
-   time-out: The default value is 60 seconds. This parameter specifies the API time-out value of OSS.

## View the bucket list { .section}

Add the following content at the end of `app.js`. Use the `listBuckets` interface to view the bucket list:

```language-js
async function listBuckets () {
  try {
    let result = await client.listBuckets();
  } catch(err) {
    console.log(err)
  }
}

listBuckets();

```

Run `node app.js` and view the result.

## View the object list { .section}

Modify `app.js`. Use the `list` interface to view the object list:

```
client.useBucket('Your bucket name');
async function list () {
  try {
    let result = client.list({
      'max-keys': 5
    })
    console.log(result)
  } catch (err) {
    consol.log (err)
  }
}
list();
```

Run `node app.js` and view the result.

## Upload an object { .section}

Modify `app.js`. Use the `put` interface to upload the object:

```language-js
client.useBucket('Your bucket name');

async function put () {
  try {
    let result = await client.put('object-name', 'local file');
    console.log(result);
   } catch (err) {
     consol.log (err);
   }
}

put();

```

## 下载一个文件 { .section}

Modify `app.js`. Use the `get` interface to download an object:

```language-js
async function get () {
  try {
    let result = await client.get('object-name', 'local file');
    console.log(result);
  } catch (err) {
    consol.log (err);
  }
}

get();

```

## Delete an object { .section}

Modify `app.js`. Use the `delete` interface to delete an object:

```language-js
async function delete () {
  try {
    let result = await client.delete('object-name');
    console.log(result);
  } catch (err) {
    console.log (err);
  }
}

delete();

```

## Learn more { .section}

-    [Manage a bucket](reseller.en-US/SDK Reference/Node. js/Manage a bucket.md#) 
-    [Upload objects](reseller.en-US/SDK Reference/Node. js/Upload objects.md#) 
-    [Download objects](reseller.en-US/SDK Reference/Node. js/Download objects.md#) 
-    [Manage objects](reseller.en-US/SDK Reference/Node. js/Manage objects.md#) 
-    [CNAME](reseller.en-US/SDK Reference/Node. js/CNAME.md#) 
-    [Use STS to access OSS](reseller.en-US/SDK Reference/Node. js/Use STS to access OSS.md#) 
-    [Set access permissions](reseller.en-US/SDK Reference/Node. js/Set ACLs.md#) 
-    [Manage lifecycle rules](reseller.en-US/SDK Reference/Node. js/Manage lifecycle rules.md#) 
-    [Set logging](reseller.en-US/SDK Reference/Node. js/Set logging.md#) 
-    [Static website hosting](reseller.en-US/SDK Reference/Node. js/Static website hosting.md#)
-    [Anti-Leech](reseller.en-US/SDK Reference/Node. js/Anti-leech.md#) 
-    [Exception](reseller.en-US/SDK Reference/Node. js/Exception handling.md#)

