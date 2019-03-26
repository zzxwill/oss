# FAQ {#concept_63401_zh .concept}

This topic describes common problems that may occur in the use of OSS Node.js SDK and the solutions for the problems.

## How to enable HTTPS access {#section_zgn_myk_lfb .section}

To enable HTTPS access, specify the value of the secure parameter to true when initializing the SDK.

## How to obtain the upload progress { .section}

You can specify the progress parameter to obtain the upload progress when performing [multipart upload](reseller.en-US//Upload objects.md#) operations.

## How to obtain the download progress { .section}

The download progress is calculated based on the size of the download traffic in OSS Node.js SDK.

## How to upload Base64-encoded images { .section}

Convert the images to File objects and upload them to the OSS server by calling APIs.

```
 function dataURLtoFile(dataurl, filename) {
    let arr = dataurl.split(','), mime = arr[0].match(/:(. *?) ;/)[1],
      bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
    while(n--){
      u8arr[n] = bstr.charCodeAt(n);
    }
    return new File([u8arr], filename, {type:mime});
  }

  let file = dataURLtoFile('<base64 content>', '');

  client.multipartUpload('<oss file name>', file).then( (res)=> {
    console.log(res)
  }).catch((err) => {
    console.log(err)
  });

```

## How to upload a file to a specified directory { .section}

You only need to add the prefix of the directory to the name of the object that you want to upload. For more information, see: [Comparison of OSS and file system](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#).

```
let OSS = require('ali-oss')
let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

client.multipartUpload('base-dir/' +'object-name', 'local-file', {
    progress: async function (p) {
      console.log('Progress: ' + p);
    }
  });
  console.log(result);
}).then((res) => {
  console.log(res)
}). catch((err) => {
  console.log(err);
});


```

## How to obtain the signed URL of an object { .section}

You can call the `signatureUrl` method to obtain the signed URL used to download an object. For more information, see the [signatureUrl](https://github.com/ali-sdk/ali-oss#user-content-signatureurlname-options) part in the OSS SDK document.

## Common errors { .section}

-    [Exception handling](reseller.en-US/SDK Reference/Node. js/Exception handling.md#) 
-    [OSS error response](../../../../../reseller.en-US/Errors and Troubleshooting/OSS error response.md#) 

