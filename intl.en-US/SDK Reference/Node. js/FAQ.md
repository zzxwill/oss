# FAQ {#concept_63401_zh .concept}

This topic lists common questions and answers.

## How to enable HTTPS access {#section_zgn_myk_lfb .section}

When you initialize the SDK, you can input the following parameters:

-   region: This parameter is the region used when you applied for the OSS service, such as `oss-cn-hangzhou`. Complete region list can be viewed in [OSS Nodes](../../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).
-   internal: Use in combination with `region`. If `internal` is specified to `true`, visit the intranet node.
-   secure: Use in combination with `region`. If `secure` is specified to `true`, use the HTTPS for access.
-   endpoint: For example, `http://oss-cn-hangzhou.aliyuncs.com`. If the `endpoint` is specified, the `region` is ignored. You can specify HTTPS or IP when using `endpoint`.

## How to obtain the upload progress { .section}

You can obtain the upload progress when using [multipart upload](reseller.en-US/SDK Reference/Node. js/Upload objects.md#).

## How to obtain the download progress { .section}

The node can calculate the download progress based on the size of the download traffic.

## How to upload Base64-encoded images { .section}

Convert the Base64 content to a File object and upload it to the OSS server on the call API.

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

## How to upload files to a specified directory { .section}

You only need to add the prefix of the specified directory to the name of the object to be uploaded, see: [Comparison of OSS and file system](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#).

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

You can call the `signatureUrl` method to obtain the download address. See [related documents](reseller.en-US/SDK Reference/Node. js/Download objects.md#).

## Common errors { .section}

-    [SDK exception log is enabled](reseller.en-US/SDK Reference/Node. js/Exception handling.md#) 
-    [OSS common errors](../../../../../reseller.en-US/Errors and Troubleshooting/OSS error response.md#) 

