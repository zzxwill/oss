# Download to local memory {#concept_lrr_qth_dhb .concept}

This topic describes how to download an object to local memory.

You can use `get` to download an object from OSS to local memory:

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function getBuffer () {
  try {
    let result = await client.get('object-name');
    console.log(result.content);
  } catch (e) {
    console.log(e);
  }
}

getBuffer();

```

