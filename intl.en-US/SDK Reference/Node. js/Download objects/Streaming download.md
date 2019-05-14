# Streaming download {#concept_e5m_jth_dhb .concept}

If you have a large object to download or it is time-consuming to download the entire object at a time, you can use streaming download. Streaming download enables you to download part of the object content each time until you download the entire object.

When you use `getStream` to download an object, `Readable Stream` is returned and used to stream the object content.

```language-js
let OSS = require('ali-oss');
let fs = require('fs');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function getStream () {
  try {
    let result = await client.getStream('object-name');
    console.log(result);
    let writeStream = fs.createWriteStream('local-file');
    result.stream.pipe(writeStream);
  } catch (e) {
    console.log(e);
  }
}

getStream()

```

