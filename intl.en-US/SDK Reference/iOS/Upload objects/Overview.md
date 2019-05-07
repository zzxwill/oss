# Overview {#concept_32060_zh .concept}

This document describes the file upload methods provided by the OSS iOS SDK.

Objects are the basic data units for user operations in OSS. The OSS iOS SDK provides the following object upload methods:

-   [Simple upload](reseller.en-US/SDK Reference/iOS/Upload objects/Simple upload.md#): supports the upload of a file up to 5 GB in size. It includes local file upload and upload from the memory.
-   [Multipart upload](reseller.en-US/SDK Reference/iOS/Upload objects/Multipart upload.md#): supports the upload of a file up to 48.8 TB in size. It applies to the upload of large files.
-   [Append upload](reseller.en-US/SDK Reference/iOS/Upload objects/Append upload.md#): supports the upload of a file up to 5 GB in size.
-   [Resumable upload](reseller.en-US/SDK Reference/iOS/Upload objects/Resumable upload.md#): supports the upload of a file up to 48.8 TB in size. It applies to the upload of large files. It supports concurrent upload and you can define the size of each part.

**Note:** For more information about the scenarios of each upload method, see the "Upload files" section in OSS Developer Guide.

After you complete the file upload, you can perform upload callback, as shown in [Upload callback](reseller.en-US/SDK Reference/iOS/Upload objects/Upload callback.md#).

