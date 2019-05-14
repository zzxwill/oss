# Download to a local file {#concept_m2q_hth_dhb .concept}

This topic describes how to download an object from OSS to a local file.

You can use get to download an object from OSS to a local file:

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function get () {
  try {
    let result = await client.get('object-name', 'local-file');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

get();

```

