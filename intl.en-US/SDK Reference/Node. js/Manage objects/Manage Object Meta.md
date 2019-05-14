# Manage Object Meta {#concept_ukc_wc3_dhb .concept}

Object Meta contains HTTP headers and User Meta. For more information, see the "[Object Meta](../../../../reseller.en-US/Developer Guide/Manage files/Manage Object Meta.md#)" section in OSS Developer Guide.

When you upload a file to OSS, you can specify the information of certain properties in addition to the content. The property information is called Object Meta. Object Meta is stored together with its object during uploads, and returned with the object when the object is downloaded.

Object Meta is stored in HTTP headers during object uploads or downloads. Therefore, Object Meta cannot contain complex characters in accordance with HTTP.

**Note:** Object Meta can only contain simple characters in the ASCII format and cannot contain line breaks. The total size of Object Meta cannot exceed 8 KB.

You can set meta to specify Object Meta when you use `put`, `putStream`, or `multipartUpload`.

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

You can also use putMeta to update Object Meta:

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

