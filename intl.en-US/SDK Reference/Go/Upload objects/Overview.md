# Overview {#concept_32147_zh .concept}

In OSS, an object is the basic unit for data operations. OSS Go SDK provides the following file upload methods:

-    [Simple upload](reseller.en-US/SDK Reference/Go/Upload objects/Simple upload.md#): supports the upload of a file up to 5 GB in size.
-    [Append upload](reseller.en-US/SDK Reference/Go/Upload objects/Append upload.md#): supports the upload of a file up to 5 GB in size.
-    [Resumable upload](reseller.en-US/SDK Reference/Go/Upload objects/Resumable upload.md#): supports the upload of a file up to 48.8 TB in size. It applies to the upload of large files. It supports concurrent upload and you can define the size of each part.
-    [Multipart upload](reseller.en-US/SDK Reference/Go/Upload objects/Multipart upload.md#): supports the upload of a file up to 48.8 TB in size. It applies to the upload of large files.

**Note:** For more information about the usage scenarios of each upload method, see “Upload files” in OSS Developer Guide.

During the file upload, you can set [Object Meta](reseller.en-US/SDK Reference/Go/Manage objects/Manage Object Meta.md#), and view the upload progress in the [upload progress bar](reseller.en-US/SDK Reference/Go/Upload objects/Progress bars.md#).

