# Delete objects {#concept_i1s_pc3_dhb .concept}

This topic describes how to delete objects.

## Delete an object {#section_bvz_qc3_dhb .section}

Use `delete` to delete an object:

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
    let result = await client.delete('object-name');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

delete();

```

## Delete multiple objects {#section_xgd_qc3_dhb .section}

Use `deleteMulti` to delete multiple objects and use quiet to specify whether to return the deletion results:

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

