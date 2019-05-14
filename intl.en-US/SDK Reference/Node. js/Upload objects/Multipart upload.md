# Multipart upload {#concept_hgg_3vb_dhb .concept}

This topic describes how to use multipart upload.

**Note:** The following sample code uses the catch syntax. Learn ES6 Promise and ES6 Async/Await on your own. For more information about how to use the SDK, see [Installation](reseller.en-US/SDK Reference/Node. js/Installation.md#).

To upload a large file, you can use `multipartUpload`. The advantage of multipart upload is to divide the upload request of a large file into multiple small requests and run each small request. This method guarantees that when some of the requests fail, you need only to upload the failed parts instead of the entire file. We recommend that you use multipart upload for files that are larger than 100 MB.

If `ConnectionTimeoutError` occurs during the use of multipartUpload, the business side must process the timeout logic on its own. You can scale in the part size, prolong the timeout period, retry the request, or analyze the specific cause by capturing `ConnectionTimeoutError`.

Relevant parameters are described as follows:

-   name \{String\}: specifies the object name.
-   file \{String|File\}: specifies the file path or the HTML5 file.
-   \[options\] \{Object\}: specifies optional parameters.
    -   \[checkpoint\] \{Object\}: specifies the checkpoint information. If this parameter is set, the upload starts from the recorded checkpoint information. If this parameter is not set, the entire file needs to be uploaded.
    -   \[parallel\] \{Number\}: specifies the number of parts that are to be uploaded simultaneously.
    -   \[partSize\] \{Number\}: specifies the size of each part.
    -   \[progress\] \{Function\}: specifies the async function mode. The callback function involves three parameters:
        -   \(percentage \{Number\}: specifies the percentage of the upload progress \(or a decimal from 0 to 1\).
        -   checkpoint \{Object\}: specifies the checkpoint information.
        -   res \{Object\}\): specifies the response to a part upload request.
    -   \[meta\] \{Object\}: specifies the Object Meta you can set in the header. The prefix of Object Meta in the header is `x-oss-meta-`.
    -   \[mime\] \{String\}: specifies the `Content-Type` value you can define in the header.
    -   \[headers\] \{Object\}: specifies other field information in the header. For more information, see [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616.html).
        -   'Cache-Control': specifies the cache mechanism that is added to HTTP request or response headers. The cache mechanism is implemented through commands. Example: `Cache-Control: public, no-cache`.
        -   'Content-Disposition': specifies the form in which the reply content is displayed. The reply content can be displayed on a Web page, or downloaded and saved locally as an attachment. Example: `Content-Disposition: somename`.
        -   'Content-Encoding': compresses the data of a specific medium type, such as `Content-Encoding: gzip`.
        -   'Expires': specifies the validity period, such as `Expires: 3600000`.

Use the following code for multipart upload:

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
   // Catch the timeout exception.
	if (e.code === 'ConnectionTimeoutError') {
	  console.log("Woops,timeout exception!") ;
	  // do ConnectionTimeoutError operation
	}
    console.log(e)
  }
}

```

progress is a progress upload callback function that is used to obtain the upload progress. progress can be an async function:

```language-js
const progress = async function (p) {
  console.log(p);
};

```

meta specifies Object Meta. You can use head to obtain Object Meta.

