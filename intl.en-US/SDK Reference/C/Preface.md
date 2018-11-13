# Preface {#concept_32131_zh .concept}

This document is written based on OSS C SDK 3.5.0.

## Compatibility {#section_qq5_ymz_kfb .section}

-   For SDK series of 3.\*.\*: compatible
-   For SDK series of 2.\*.\*:

    -   Windows: compatible
    -   Linux API: compatible except for the linked list \(aos\_list\_t\) traversal interface.
        -   os\_list\_for\_each\_entry
        -   aos\_list\_for\_each\_entry\_reverse
        -   aos\_list\_for\_each\_entry\_safe
        -   aos\_list\_for\_each\_entry\_safe\_reverse
-   For SDK series of 1.0.0: incompatible for only the following APIs

    -   oss\_request\_options\_t
    -   oss\_get\_object\_to\_buffer
    -   oss\_get\_object\_to\_file
    -   oss\_get\_object\_to\_buffer\_by\_url
    -   oss\_get\_object\_to\_file\_by\_url
    -   oss\_init\_multipart\_upload
    -   oss\_complete\_multipart\_upload
-   For SDK series of 0.0.\*: incompatible

## SDK source code { .section}

For the Java SDK source code, see [GitHub](https://github.com/aliyun/aliyun-oss-c-sdk/tree/master).

## Code samples { .section}

OSS SDK for C provides a variety of code samples for your reference or use. The following table describes the content of code samples:

|Sample file|Content|
|:----------|:------|
| [oss\_put\_object\_sample](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_put_object_sample.c) | [Upload objects](reseller.en-US/SDK Reference/C/Upload objects/Overview.md#) |
| [oss\_get\_object\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_get_object_sample.c) | [Download objects](reseller.en-US/SDK Reference/C/Download objects/Overview.md#) |
| [oss\_append\_object\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_append_object_sample.c) | [Append upload](reseller.en-US/SDK Reference/C/Upload objects/Append upload.md#)|
| [oss\_multipart\_upload\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_multipart_upload_sample.c) | [Multipart upload](reseller.en-US/SDK Reference/C/Upload objects/Multipart upload.md#)|
| [oss\_resumable\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_resumable_sample.c) | [Resumable upload](reseller.en-US/SDK Reference/C/Upload objects/Resumable upload.md#), [Resumable download](reseller.en-US/SDK Reference/C/Download objects/Resumable download.md#) |
| [oss\_head\_object\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_head_object_sample.c) | [Manage the metadata of an object](reseller.en-US/SDK Reference/C/Manage objects/Manage Object Meta.md#) |
| [oss\_list\_object\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_list_object_sample.c) |[List objects](reseller.en-US/SDK Reference/C/Manage objects/List objects.md#) |
| [oss\_delete\_object\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_delete_object_sample.c) | [Delete an object](reseller.en-US/SDK Reference/C/Manage objects/Delete objects.md#) |
| [oss\_callback\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_callback_sample.c) | [Upload callback](reseller.en-US/SDK Reference/C/Upload objects/Upload callback.md#) |
| [oss\_progress\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_progress_sample.c) |[Progress bar](reseller.en-US/SDK Reference/C/Upload objects/Upload progress bars.md#), [Progress bar](reseller.en-US/SDK Reference/C/Download objects/Download progress bars.md#) |
| [oss\_crc\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_crc_sample.c) |CRC check during upload or download|
| [oss\_image\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_image_sample.c) | [Image processing](reseller.en-US/SDK Reference/C/Image processing.md#) |

