# Upload a local file {#concept_uxl_2vb_dhb .concept}

This topic describes how to upload a local file to OSS.

**Note:** The following sample code uses the catch syntax. Learn ES6 Promise and ES6 Async/Await on your own. For more information about how to use the SDK, see [Installation](reseller.en-US/SDK Reference/Node. js/Installation.md#).

You can use put to upload a local file to OSS:

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
    let result = await client.put('object-name', 'local-file');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

put();

```

