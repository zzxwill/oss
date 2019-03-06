# FAQ {#concept_64057_zh .concept}

This topic describes common questions and answers.

## How to call STS {#section_ljz_42m_lfb .section}

A browser is an untrusted environment. It brings an extremely high risk if you store an AccessKeyID and AccessKeySecret in a browser. We recommend that you use the [STS](https://help.aliyun.com/document_detail/56286.html) mode

for OSS interface calls in a browser environment.

After obtaining the STS token, you can initialize the SDK.

```
<script type="text/javascript">
  $.ajax("http://your_sts_server/",{method: 'GET'},function (err, result) {
    let client = new OSS({
      accessKeyId: result.AccessKeyId,
	  accessKeySecret: result.AccessKeySecret,
	  stsToken: result.SecurityToken,
	  endpoint: '<oss endpoint>',
	  bucket: '<Your bucket name>'
    });
  });
</script>

```

## How to enable HTTPS access { .section}

When you initialize the SDK, you can input the following parameters:

-   region: This parameter is the region used when you apply for the OSS service, such as `oss-cn-hangzhou`. Complete region lists can be viewed in [Regions and endpoints](../../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).
-   internal: Used in combination with `region`. If `internal` is set to `true`, visit the intranet node.
-   secure: Used in combination with `region`. If `secure` is set to `true`, use the HTTPS for access.
-   endpoint: For example, `http://oss-cn-hangzhou.aliyuncs.com`. If `endpoint` is specified, the `region` is ignored. You can specify the HTTPS or IP address when using `endpoint`.

## How to solve the cross-domain issues when using a browser { .section}

Set CORS attributes of buckets before using SDK in a browser. For more information, see bucket settings in [Quick start](reseller.en-US/SDK Reference/Browser.js/Quick start.md#).

## How to set the user-defined data \(meta\), file type \(mime\), and request header \(header\) of the file to be uploaded { .section}

For more information, see [Multipart upload](reseller.en-US/SDK Reference/Browser.js/Upload objects.md#).

## Instruction for resumable upload on browser side { .section}

Save the checkpoint to a browser localStorage. You can directly use the checkpoint parameter next time to achieve resumable upload.

## How to obtain the upload progress { .section}

You can obtain upload process when using multipart upload. For more information, see [Upload objects](reseller.en-US/SDK Reference/Browser.js/Upload objects.md#).

## How to obtain the download progress { .section}

The browser does not provide download progress. You can call the `signatureUrl` method to obtain the download address. For more information, see [Download objects](reseller.en-US/SDK Reference/Browser.js/Download objects.md#).

## How to upload files to a specified directory { .section}

Add the specified directory prefix to the object name to be uploaded. For more information, see [Comparison between OSS and file system](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#).

```
let OSS = require('ali-oss')
let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

client.multipartUpload('base-dir/' +'object-key', 'local-file', {
    progress: async function (p) {
      console.log('Progress: ' + p);
    }
  });
  console.log(result);
}).catch((err) => {
  console.log(err);
});


```

## How to upload a Base64-encoded image { .section}

Base64 first transcodes images into specified formats, and then calls OSS Upload interface to upload the images. For more information, see [Github examples](https://github.com/ali-sdk/ali-oss/blob/master/example/src/main.js#L109).

```
/**
 * base64 to file
 * @param dataurl   base64 content
 * @param filename  set up a meaningful suffix, or you can set mime type in options
 * @returns {File|*}
 */
const dataURLtoFile = function dataURLtoFile(dataurl, filename) {
  const arr = dataurl.split(',');
  const mime = arr[0].match(/:(. *?) ;/)[1];
  const bstr = atob(arr[1]);
  let n = bstr.length;
  const u8arr = new Uint8Array(n);
  while (n--) {
    u8arr[n] = bstr.charCodeAt(n);
  }
  return new Blob([u8arr], { type: mime });// if env support File, also can use this: return new File([u8arr], filename, { type: mime });
};

// client indicates OSS client instances
const uploadBase64Img = function uploadBase64Img(client) {
  // base64 format content
  const base64Content = 'data:image:xxxxxxxxxxxxx';
  const filename =  'img.png';
  const imgfile = dataURLtoFile(base64Content, filename);
  //key indicates uploaded object key and imgFile indicates the returned images after processing by dataURLtoFile
  client.multipartUpload(key, imgfile).then((res) => {
    console.log('upload success: %j', res);
  }).catch((err) => {
    console.error(err);
  });
};

```

## How to limit the size of the uploaded file { .section}

The size \(in bytes\) of the uploaded file can be obtained in the browser according to document.getElementById\(“file”\).files\[0\].size. See the web post request.

## How to obtain the signed URL of an object { .section}

You can call the `signatureUrl` method to obtain the download address. For more information, see [Download objects](reseller.en-US/SDK Reference/Browser.js/Download objects.md#).

## How to use a signed URL generated by SDK to upload resources { .section}

Signed URLs are often used to authorize third parties to download and upload resources. For the download operations, see the answer for the next question. The signatureUrl API provided by SDK is used to return a signed URL and users can use this URL to upload or download resources directly. For more information about how to upload resources using signed URLs, see [Upload resources using signed URLs](https://github.com/ali-sdk/ali-oss/blob/master/example/src/main.js).

## How to upload resources to OSS servers using Form-based File Upload { .section}

For more information, see [Overview of direct transfer on Web client](../../../../../reseller.en-US/Best Practices/Direct upload to OSS from Web/Overview of direct transfer on Web client.md#).

## Common errors { .section}

-    [JS-SDK exception](reseller.en-US/SDK Reference/Browser.js/Exception handling.md#) 
-   [OSS error response](../../../../../reseller.en-US/Errors and Troubleshooting/OSS error response.md#) 

