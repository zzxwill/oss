# Upload objects {#concept_64047_zh .concept}

You can upload a file to OSS using any of the following methods:

-   Upload Blob data
-   Resumable upload

## Upload Blob Data { .section}

Binary Large Object \(Blob\) indicates a large object of the binary type. The concept of blob is used in some databases. For example, the BLOB type used in MySQL indicates a container for binary data.

You can also use the `put` interface to upload content in a Blob to OSS:

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function putBlob () {
  try {
    let result = await client.put('object-key', new Blob(['content'],{ type: 'text/plain' }));
    console.log(result);
  } catch (e) {
    conosle.log(e);
  }
}
putBlob();

```

## Resumable upload {#section_snl_fxt_4fb .section}

When the file to be uploaded is large, you can use the multipartUpload interface for multipart upload. Multipartupload is to divide a large request into multiple small requests for execution. As a result, when some of the requests fail,you do not need to upload the entire file again, but only to upload the failed parts. Generally for a file larger than 100 MB, we recommend that you use the preceding multipart upload approach and create a new OSS instance before each multipart upload.

If a `ConnectionTimeoutError` error occurs when you use the multipartUpload interface, you must construct your own method to handle time-out issues. You can reduce the part size, increase the time-out period and allowed retry times, or catch the `ConnectionTimeoutError` error and return it to the user. For more information, see [network error handling](../../../../../reseller.en-US/Errors and Troubleshooting/Network connection timeout handling.md#).

For the usage of multipartUpload API, see [MultipartUpload](../../../../../reseller.en-US/API Reference/Multipart upload operations/Introduction.md#).

Related parameters:

-   name \{String\}: Object name
-   file \{File\}: HTML5 Web File or Blob data
-   \[options\] \{Object\}: Additional parameters
    -   \[checkpoint\] \{Object\}: Endpoint checkpoint used in resumable upload. If this parameter is set, the upload starts from the endpoint. If it is not set, the upload restarts.
        -   file \{File\}: File object selected by the user. Users must manually set this parameter if the browser is restarted.
        -   name \{String\}: Uploaded object key
        -   fileSize \{Number\}: File size
        -   partSize \{Number\}: Part size
        -   uploadId \{String\}: Upload Id
        -   doneParts \{Array\}: Array of uploaded parts, including the following objects:
            -   number \{Number\}: Part number
            -   etag \{String\}: Part etag
    -   \[parallel\] \{Number\}: Number of parts uploaded simultaneously
    -   \[partSize\] \{Number\}: Part size
    -   \[progress\] \{Function\}: A generator function or a thunk. The callback function contains the following three parameters: `function`, `async`, and `promise`.
        -   \(percentage \{Number\}: Percetage of upload progress \(a decimal range from 0 to 1\)
        -   checkpoint \{Object\}: Endpoint checkpoint
        -   res \{Object\}\): Response returned after a single part is successfully uploaded
    -   \[meta\] \{Object\}: Header meta inforamtion defined by users with a prefix `x-oss-meta-` 
    -   \[mime\] \{String\}: Custom `Content-Type header` 
    -   \[headers\] \{Object\}: Extra headers. See [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616.html) for more information.
        -   ‘Cache-Control’: General header used to implement cache mechanisms by specifying a command in HTTP requests and responses. For example: `Cache-Control: public, no-cache` 
        -   ‘Content-Disposition’: Used to indicate the disposition form of a response, which can be an internal reference \(a part of a webpage or a page\) or an attachment downloaded and saved locally. For example: `Content-Disposition: somename` 
        -   ‘Content-Encoding’: Used to compress data of specific media type. For example: `Content-Encoding: gzip` 
        -   ‘Expires’: Expiration time. For example: `Expires: 3600000` 
    -   \[callback\] \{Object\}: Callback settings For more information, see [Callback]().
        -   url \{String\}: The address of the callback server communicated with the OSS server. It corresponds to callbackUrl in the CallBack parameter. Required
        -   body \{String\}: The value of callback request. It is in the JSON format and corresponds to callbackBody in the Callback parameter. Required
        -   \[host\] \{String\}: The value of the Host header in the callback request. It corresponds to callbackHost inthe CallBack parameter.
        -   \[contentType\] \{String\}: The Content-Type of the callback request. It corresponds to callbackBodyType in the CallBack parameter.
        -   \[customValue\] \{Object\}: Custom parameters in the callback request. It corresponds to callback-var in the CallBack parameter.

Example:

1.  The upload progress is recorded. When the upload is initiated again, the recorded checkpoint parameter is passed in.
2.  We recommend that you create a new OSS instance for each multipart upload task.

```language-js
let OSS = require('ali-oss')

let ossConfig = {
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
}

let client = new OSS(ossConfig);

let tempCheckpoint;

// Define the upload method
async function multipartUpload () {
  try {
    let result = await client.multipartUpload('object-key', 'local-file', { 
      progress: async function (p, checkpoint) {
        // Record the checkpoint. If you close the browser and continue the upload after restarting the browser, the upload cannot be continued. For more information, see the description of file objects.
        tempCheckpoint = checkpoint;
      }
      meta: { year: 2017, people: 'test' },
      mime: 'image/jpeg'
   })
  } catch(e){
    console.log(e);
  }
}

// Start uploading
multipartUpload();

// Pause multipart upload
client.cancel();

// Resume multipart upload
let resumeclient = new OSS(ossConfig);
async function resumeUpload () {
  try {
    let result = await resumeclient.multipartUpload('object-key', 'local-file', {
	progress: async function (p, checkpoint) {
          tempCheckpoint = checkpoint;
        },
        checkpoint: tempCheckpoint
        meta: { year: 2017, people: 'test' },
        mime: 'image/jpeg'
  })
  } catch (e) {
    console.log(e);
  }
}

resumeUpload();

```

The preceding `progress` parameter is a progress callback function used to get the upload progress.

```language-js
const progress = async function progress(p, checkpoint) {
  console.log(p)
};

```

The preceding `meta` parameter is a user-defined metadata. You can obtain the meta value of the object using the head API, but the meta header must be set properly in the Exposed Headers of the Cross-region Settings in the console, as shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22573/155186563813702_en-US.png)

Returned results of successful requests are as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22573/155186563813703_en-US.png)

You can add `callback` in the option parameter to send callback information.

```language-javascript
callback: {
  url: 'http://oss-demo.aliyuncs.com:23450',
  host: 'oss-cn-hangzhou.aliyuncs.com',
  /* eslint no-template-curly-in-string: [0] */
  body: 'bucket=${bucket}&object=${object}&var1=${x:var1}',
  contentType: 'application/x-www-form-urlencoded',
  customValue: {
    var1: 'value1',
    var2: 'value2',
  },
},

```

