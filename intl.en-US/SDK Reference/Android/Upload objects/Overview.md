# Overview {#concept_32047_zh .concept}

This topic describes the methods provided by OSS Android SDK to upload objects.

In OSS, an object is the basic unit for data operations. OSS Android SDK provides the following object upload methods:

-   [Simple upload](reseller.en-US/SDK Reference/Android/Upload objects/Simple upload.md#): Supports uploading local files and binary byte\[\] arrays that are smaller than or equal to 5 GB.
-   [Append upload](reseller.en-US/SDK Reference/Android/Upload objects/Append upload.md#): Supports uploading files that are smaller than or equal to 5 GB.
-   [Resumable upload](reseller.en-US/SDK Reference/Android/Upload objects/Resumable upload.md#): Supports concurrent resumable upload tasks and custom part size. We recommend you use this method when uploading files that are larger than 5 GB and smaller than or equal to 48.8 TB.

**Note:** For more information about the application scenarios for the preceding upload methods, see Upload objects in the OSS Developer Guide.

When uploading a file, you can view the upload progress through the [progress bar](reseller.en-US/SDK Reference/Android/Upload objects/Progress bar.md#). After a file is uploaded, you can perform [upload callback](reseller.en-US/SDK Reference/Android/Upload objects/Upload callback.md#).

