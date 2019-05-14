# Overview {#concept_32072_zh .concept}

Objects are the basic unit for data operations in OSS. The OSS Node.js SDK provides the following methods to upload objects:

-   [Local file upload](reseller.en-US/SDK Reference/Node. js/Upload objects/Upload a local file.md#): supports the upload of a file up to 5 GB in size.
-   [Upload from local memory](reseller.en-US/SDK Reference/Node. js/Upload objects/Upload from local memory.md#): supports the upload of a file up to 5 GB in size.
-   [Streaming upload](reseller.en-US/SDK Reference/Node. js/Upload objects/Streaming upload.md#): supports any file sizes.
-   [Multipart upload](reseller.en-US/SDK Reference/Node. js/Upload objects/Multipart upload.md#): supports the upload of a file up to 48.8 TB in size. This method is suitable for the upload of large files.
-   [Resumable upload](reseller.en-US/SDK Reference/Node. js/Upload objects/Resumable upload.md#): supports the upload of a file up to 48.8 TB in size. This method is suitable for the upload of large files. It supports concurrent upload and you can define the size of each part.

**Note:** For more information about the scenarios of each upload method, see the "Upload files" section in OSS Developer Guide.

