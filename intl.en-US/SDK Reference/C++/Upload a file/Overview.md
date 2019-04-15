# Overview {#concept_32136_zh .concept}

Objects are the basic unit for data operations in OSS. The OSS C++ SDK provides the following object upload methods:

-   [Simple upload](reseller.en-US/SDK Reference/C++/Upload a file/Simple upload.md#): supports the upload of a file up to 5 GB in size. It includes upload from memory and local file upload.
-   [Append upload](reseller.en-US/SDK Reference/C++/Upload a file/Append upload.md#): supports the upload of a file up to 5 GB in size.
-   [Resumable upload](reseller.en-US/SDK Reference/C++/Upload a file/Resumable upload.md#): supports the upload of a file up to 48.8 TB in size. It applies to the upload of large files. It supports concurrent upload and you can define the size of each part.
-    [Multipart upload](reseller.en-US/SDK Reference/C++/Upload a file/Multipart upload.md#): supports the upload of files up to 48.8 TB in size. It applies to the upload of large files.

**Note:** For more information about the scenarios of each upload method, see the "Upload files" section in OSS Developer Guide.

During the file upload, you can set Object Meta and view upload progress, as shown in [Configure Object Meta](reseller.en-US/SDK Reference/C++/Manage an object/Manage Object Meta.md#) and [Use progress bars](reseller.en-US/SDK Reference/C++/Upload a file/Use a progress bar.md#). After you upload the file, you can perform upload callback, as shown in [Upload callback](reseller.en-US/SDK Reference/C++/Upload a file/Upload callback.md#).

