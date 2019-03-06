# Upload objects {#concept_32072_zh .concept}

This topic describes how to upload files.

You can upload a file to OSS through any of the following methods:

**Note:** To know the syntax of catch, learn es6 promise and async/await. For the usage of SDK, click [here](reseller.en-US/SDK Reference/Node. js/Installation.md#).

-   Upload a local file
-   Stream upload
-   Upload content in the buffer
-   Multipart upload
-   Resumable upload

## Upload a local file {#section_ys4_4rk_lfb .section}

You can use the put interface to upload a local file to OSS:

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
    let result = await client.put('object-name', 'local-file');
    console.log(result);
  } catch (e) {
  	console.log(er);
  }
}

put();

```

## Stream upload { .section}

You can use the putStream interface to upload the content in a stream. The stream parameter can be any object that implements Readable Stream, including the object stream and network stream. When you use the putStream interface, SDK initiates a chunked encoding HTTP PUT request by default. If you specify the contentLength parameter in options, the chunked encoding is not used.

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

## Upload content in the buffer { .section}

You can upload the object content in the buffer to OSS through the `put` interface:

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<your access key id>',
  Acceskeysecret: '<Your acceskeysecret> ',
  bucket: 'Your bucket name'
});

async function putBuffer () {
  try {
    let result = await client.put('object-name', new Buffer('hello world'));
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

putBuffer();

```

## Multipart upload { .section}

When the file to be uploaded is large, you can use the `multipartUpload` interface for multipart upload. The advantage of multipart upload is to divide a large request into multiple small requests for execution. As a result, when some of the requests fail, you do not need to upload the entire file but, you can upload only the failed parts. Generally for an object larger than 100 MB, we recommend the preceding multipart upload approach.

If a `ConnectionTimeoutError` error occurs when you use the multipartUpload interface, you must construct your own method to handle time-out issues. You can reduces the part size, increase the time-out period and allowed retry times, or catch the `ConnectionTimeoutError` and return it to the user.

Related parameters:

-   name \{String\}: Object name
-   file \{String|File\} file path or HTML5 Web File
-   \[options\] \{Object\}: Optional parameter
    -   \[checkpoint\] \{Object\}: Endpoint checkpoint used in resumable upload. If this parameter is set, the upload starts from the endpoint. If it is not set, the upload restarts.
    -   \[parallel\] \{Number\}: Number of parts uploaded simultaneously
    -   \[partSize\] \{Number\}: Part size
    -   \[progress\] \{Funtion\}: A generator function or a thunk. The callback function contains the following three parameters:
        -   \(percentage \{Number\}: Percetage of upload progress \(a decimal range from 0 to 1\)
        -   checkpoint \{Object\}: Endpoint checkpoint
        -   res \{Object\}\): Response returned after a single part is successfully uploaded
    -   \[meta\] \{Object\}: Header meta inforamtion defined by users with a prefix `x-oss-meta-` 
    -   \[mime\] \{String\}: Custom `Content-Type header` 
    -   \[headers\] \{Object\} extra headers, detail see [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616.html) 
        -   ‘Cache-Control’: General header used to implement cache mechanisms by specifying a command in HTTP requests and responses. For example: `Cache-Control: public, no-cache` 
        -   ‘Content-Disposition’: Used to indicate the disposition form of a response, which can be an internal reference \(a part of a webpage or a page\) or an attachment downloaded and saved locally. For example: `Content-Disposition: somename` 
        -   ‘Content-Encoding’: Used to compress data of specific media type. For example: `Content-Encoding: gzip` 
        -   ‘Expires’: Expiration time. For example: `Expires: 3600000` 

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function multipartUpload () {
  try {
    let result = await client.multipartUpload('object-name', 'local-file', {
    progress,
	meta: {
	  year: 2017,
	  people: 'test'
	}
  });
  console.log(result);
  let head = await client.head('object-name');
  console.log(head);
  } catch (e) {
   // Catch time-out exceptions.
	if (e.code === 'ConnectionTimeoutError') {
	  console.log("Woops, time-out error occurs!") ;
	  // do ConnectionTimeoutError operation
	}
    console.log(e)
  }
}

```

The preceding progress parameter is a progress callback function used to get the upload progress. The progress parameter can be a generator function \(function\*\), or a thunk:

```language-js
const progress = async function (p) {
  console.log(p);
};

```

The preceding meta parameter is the metadata defined by users, which can obtain the metadata of object through the head interface.

## Resumable upload { .section}

Multipart upload provides the progress parameter to allow users to pass a progress callback. In the callback SDK, the proportion and checkpoint information of the currently successful uploads are used as parameters. To achieve resumable upload, you can save the checkpoint information during the upload process. In case of errors, you can pass the saved checkpoint as parameters to multipartUpload and the upload resumes from the last failed part.

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
  // retry 5 times
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

The preceding code saves the checkpoint in the variable. If the program crashes, the information is lost. You can save information in an object and read the checkpoint information from the object after the project gets restarted.

