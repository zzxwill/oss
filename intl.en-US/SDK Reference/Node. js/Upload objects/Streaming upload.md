# Streaming upload {#concept_txq_hvb_dhb .concept}

This topic describes how to perform streaming upload.

**Note:** The following sample code uses the catch syntax. Learn ES6 Promise and ES6 Async/Await on your own. For more information about how to use the SDK, see [Installation](reseller.en-US/SDK Reference/Node. js/Installation.md#).

You can use putStream to upload the content of a stream. You can set stream to specify the types of Readable Stream, such as file stream and network stream. When you use putStream, the SDK initiates an HTTP PUT request for chunked encoding. If contentLength is specified in options, chunked encoding is not used.

```language-js
let OSS = require('ali-oss');
let fs = require('fs');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function putStream () {
  try {
  // use 'chunked encoding'
  let stream = fs.createReadStream('local-file');
  let result = await client.putStream('object-name', stream);
  console.log(result);

  // don't use 'chunked encoding'
  let stream = fs.createReadStream('local-file');
  let size = fs.statSync('local-file').size;
  let result = await client.putStream(
    'object-name', stream, {contentLength: size});
  console.log(result);
  } catch (e) {
    console.log(e)
  }
}

putStream();

```

