# Simple upload {#concept_bws_3bb_5db .concept}

Simple upload refers to the upload of a single object by using the Put Object method in the OSS API. Simple upload is applicable to the scenario where a single HTTP request interaction completes an upload, for example, the upload of a small object.

## Set object metadata when uploading an object {#section_mr2_nbb_5db .section}

When using the simple upload, you can set object metadata that describes the object, for example, Content-Type and other standard HTTP headers. You can also set user-defined information. For more information, see [Object metadata](reseller.en-US/Developer Guide/Manage files/Object Meta.md#).

## Upload restrictions {#section_ngq_qbb_5db .section}

-   The maximum size of a single object is 5 GB.
-   The naming conventions of objects are as follows:
    -   Object names must use UTF-8 encoding.
    -   Object names must be at least 1 byte and no more than 1,023 bytes in length.
    -   Object names cannot start with a backslash \( \\ \) or a forward slash \( / \).

## Upload large objects {#section_irn_5bb_5db .section}

In the single upload, objects are uploaded through a single HTTP request. Therefore, it may take a long time for you to upload large objects. If you experience bad network connection, the upload has a high failure rate. For objects larger than 5 GB, we recommend that you use [multipart upload](reseller.en-US/Developer Guide/Upload files/Multipart upload.md#).

## Security and authorization {#section_arr_vbb_5db .section}

To prevent unauthorized third parties from uploading objects to your bucket, OSS provides access control both on the bucket level and on the object level. For more information, see [Access control](reseller.en-US/Developer Guide/Access and control/Access control.md#). OSS also provides account-level authorization for third-party uploads. For more information, see [Authorized third-party uploads](reseller.en-US/Developer Guide/Upload files/Authorized third-party upload.md#).

## Further operations {#section_emr_xbb_5db .section}

After uploading objects to OSS, you may want to:

-   Initiate a callback request to a specified application server. For more information, see [Upload callback](reseller.en-US/Developer Guide/Upload files/Upload callback.md#).
-   Process the uploaded images. For more information, see [Image processing](../../../../reseller.en-US/Image Processing Guide/Image processing.md#).

## Reference {#section_nvr_ybb_5db .section}

-   API: [PutObject](../../../../reseller.en-US/API Reference/Object operations/PutObject.md#)
-   Console: [Upload objects](../../../../reseller.en-US/Console User Guide/Manage objects/Upload objects.md#)

## Best practices {#section_fsv_zbb_5db .section}

-   [RAM and STS User Guide](../../../../reseller.en-US/Best Practices/Access control/Overview.md#)
-   [Web client direct data transfer and upload callback](../../../../reseller.en-US/Best Practices/Direct upload to OSS from Web/Overview of direct transfer on Web client.md#)

