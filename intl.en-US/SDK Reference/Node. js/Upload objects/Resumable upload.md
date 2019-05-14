# Resumable upload {#concept_cwz_3vb_dhb .concept}

You can use resumable upload to split a file you want to upload into several parts and upload them simultaneously. After all parts are uploaded, you can combine these parts into a complete object. Thus, you can complete uploading the entire object.

**Note:** The following sample code uses the catch syntax. Learn ES6 Promise and ES6 Async/Await on your own. For more information about how to use the SDK, see [Installation](reseller.en-US/SDK Reference/Node. js/Installation.md#).

For more information about resumable upload, see the "[Resumable upload](../../../../reseller.en-US/Developer Guide/Upload files/Multipart upload and resumable upload.md#)" section in OSS Developer Guide.

Multipart upload provides progress to facilitate making upload progress callback requests. The SDK uses the upload progress and the checkpoint information as parameters in the callback requests. To implement resumable upload, you can save the checkpoint information during the upload process. When an error occurs, you can pass the saved checkpoint information as a parameter to multipartUpload. The upload continues from where it failed earlier.

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

let checkpoint;
async function resumeUpload() {
  // Set the number of retry attempts to 5.
  for (let i = 0; i < 5; i++) {
    try {
      const result = await client.multipartUpload('object-name', filePath, {
        checkpoint,
        async progress(percentage, cpt) {
          checkpoint = cpt;
        },
      });
      console.log(result);
      break; // break if success
    } catch (e) {
      console.log(e);
    }
  }
}

resumeUpload();

```

The preceding code saves the checkpoint information in the variable. If the program stops responding, the checkpoint information is lost. We recommend that you save the checkpoint information in a file. After the program restarts, the checkpoint information can be read from the file.

