# Overview {#concept_32013_zh .concept}

This topic is the overview of object upload in OSS Java SDK.

In OSS, an object is the basic unit for data operations. OSS Java SDK provides the following file upload methods:

-   [Simple upload](reseller.en-US/SDK Reference/Java/Upload objects/Simple upload.md#): supports the upload of a file up to 5 GB in size. It includes streaming upload and file upload.
-    [Form upload](reseller.en-US/SDK Reference/Java/Upload objects/Form upload.md#): supports the upload of a file up to 5 GB in size.
-    [Append upload](reseller.en-US/SDK Reference/Java/Upload objects/Append upload.md#): supports the upload of a file up to 5 GB in size.
-    [Resumable upload](reseller.en-US/SDK Reference/Java/Upload objects/Resumable upload.md#): supports the upload of a file up to 48.8 TB in size. It applies to the upload of large files. It supports concurrent upload and you can define the size of each part.
-   [Multipart upload](reseller.en-US/SDK Reference/Java/Upload objects/Multipart upload.md#): supports the upload of a file up to 48.8 TB in size. It applies to the upload of large files.

**Note:** For more information about the usage scenarios of each upload method, see “Upload files” in OSS Developer Guide.

During the file upload, you can set [Object Meta](reseller.en-US/SDK Reference/Java/Manage objects/Manage Object Meta.md#), and view the upload progress in the [upload progress bar](reseller.en-US/SDK Reference/Java/Upload objects/Upload progress bars.md#). After you complete the file upload, you can perform [upload callback](reseller.en-US/SDK Reference/Java/Upload objects/Upload callback.md#).

