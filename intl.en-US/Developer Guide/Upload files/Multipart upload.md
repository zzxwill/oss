# Multipart upload {#concept_wzs_2gb_5db .concept}

## Use cases {#section_y4d_tgb_5db .section}

For the object larger than 5 GB, you can use the multipart upload to split it into multiple data blocks \(called parts in OSS\) and upload them separately. When you have uploaded all parts,  OSS constructs the object from the uploaded parts.

We recommend that you use the multipart upload in the following scenarios:

-   Poor network connectivity: If the upload of one part fails, you can re-upload only the failed part instead of all parts.
-   Resumable upload required: An upload in progress can be paused and resumed at any time.
-   Upload acceleration: Multiple parts can be uploaded concurrently to speed up the process.
-   Streaming upload: Objects of unknown sizes can be uploaded at any time. This scenario is common in industry applications such as video surveillance.

## Workflow {#section_mzy_xgb_5db .section}

The workflow of the multipart upload is shown as follows:

1.  You split the object into multiple parts.
2.  You initiate a multipart upload task. For more information, see \([InitiateMultipartUpload](../intl.en-US/API Reference/Multipart upload operations/InitiateMultipartUpload.md#)\).
3.  You upload the parts one by one or concurrently. For more information, see \([UploadPart](../intl.en-US/API Reference/Multipart upload operations/UploadPart.md#)\).
4.  After all the parts are uploaded, OSS combines them into the original object. For more information, see \([CompleteMultipartUpload](../intl.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#)\).

![](images/1058_en-US.png)

When you use the multipart upload, take the following into consideration:

-   All the parts, except the last one, must not be smaller than 100 KB. Otherwise, the call to the [CompleteMultipartUpload](../intl.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#) interface fails.
-   After the object is split into parts, the parts are ordered by the partNumbers specified during the upload.  The upload speed does not correlate to the number of parts uploaded concurrently, because both the network conditions and the device load must be considered.
-   By default, when the upload is complete but the call to the [CompleteMultipartUpload](../intl.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#) interface fails, the uploaded parts will not be deleted automatically. You can call the [AbortMultipartUpload](../intl.en-US/API Reference/Multipart upload operations/AbortMultipartUpload.md#) interface to terminate the upload and save the storage space. To automatically delete the uploaded parts, see [Manage object lifecycle](intl.en-US/Developer Guide/Managing Objects/Manage object lifecycle.md#).

## Resumable upload {#section_p5x_khb_5db .section}

The uploaded parts will not disappear unless you delete them. Therefore, the multipart upload can be considered as the resumable upload.

If the system crashes during a multipart upload, you can resume the upload by using the [ListMultipartUploads](../intl.en-US/API Reference/Multipart upload operations/ListMultipartUploads.md#) and the [ListParts](../intl.en-US/API Reference/Multipart upload operations/ListParts.md#)interface to list the uploaded parts in each task. This allows uploads to be resumed from the last uploaded part.  The same logic applies to pausing and resuming uploads.

The multipart upload is particularly well suited for the data transfer between mobile devices and the large file upload.

## Restrictions {#section_cxd_mhb_5db .section}

-   The maximum size of an object is determined by the size of parts. The multipart upload supports a maximum of 10,000 parts,  and each part must be at least 100 KB \(except for the last part, which may be smaller\) and no more than 5 GB. Therefore, the object size must not exceed 48.8 TB.
-   The naming conventions of objects are as follows:
    -   Object names must use UTF-8 encoding.
    -   Object names must be at least 1 byte and no more than 1,023 bytes in length.
    -   Object names cannot start with a backslash \( \\ \) or a forward slash \( / \).

## Security and authorization {#section_bt3_4hb_5db .section}

To prevent unauthorized third parties from uploading objects to your bucket, OSS provides access control both on the bucket level and on the object level. For more information, see [Access control](intl.en-US/Developer Guide/Access and control/Access control.md#). OSS also provides account-level authorization for third-party uploads. For more information, see [Authorized third-party uploads](intl.en-US/Developer Guide/Upload files/Authorized third-party upload.md#).

## Further operations {#section_un3_phb_5db .section}

-   Initiate a callback request to a specified application server. For more information, see [Upload callback](intl.en-US/Developer Guide/Upload files/Upload callback.md#).
-   Process the uploaded data. 
-   For more information, see [Cloud data processing](intl.en-US/Developer Guide/Cloud data processing.md#).

## Usage {#section_p4k_qhb_5db .section}

-   API：
    -   [MultipartUpload](../intl.en-US/API Reference/Multipart upload operations/Introduction.md#)
    -   [InitiateMultipartUpload](../intl.en-US/API Reference/Multipart upload operations/InitiateMultipartUpload.md#)
    -   [UploadPart](../intl.en-US/API Reference/Multipart upload operations/UploadPart.md#)
    -   [UploadPartCopy](../intl.en-US/API Reference/Multipart upload operations/UploadPartCopy.md#)
    -   [CompleteMultipartUpload](../intl.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#)
    -   [AbortMultipartUpload](../intl.en-US/API Reference/Multipart upload operations/AbortMultipartUpload.md#)
    -   [ListMultipartUploads](../intl.en-US/API Reference/Multipart upload operations/ListMultipartUploads.md#)
    -   [ListParts](../intl.en-US/API Reference/Multipart upload operations/ListParts.md#)
-   SDK：Java SDK-[MultipartUpload](https://help.aliyun.com/document_detail/32013.html) in Upload objects

## Best practices {#section_gn4_shb_5db .section}

-   [RAM and STS best practices](../intl.en-US/Best Practices/Access control/Overview.md#)
-   [Web client direct upload](../intl.en-US/Best Practices/Direct upload to OSS from Web/Overview of direct transfer on Web client.md#)

## Reference links: {#section_apy_thb_5db .section}

-   [Upload callback](intl.en-US/Developer Guide/Upload files/Upload callback.md#)
-   [Introduction to mobile-side development upload scenario](intl.en-US/Developer Guide/Access OSS/OSS-based app development.md#)
-   [Download after upload](intl.en-US/Developer Guide/Download Files/Simple download.md#)
-   [Cloud Processing after uploading images](intl.en-US/Developer Guide/Image Processing.md#)
-   [Cloud Processing after uploading audio and video files](intl.en-US/Developer Guide/Cloud data processing.md#)
-   [Upload Secure Access Control](intl.en-US/Developer Guide/Access and control/Access control.md#)
-   [Authorized third-party uploads](intl.en-US/Developer Guide/Upload files/Authorized third-party upload.md#)
-   [Copy and delete files after upload](intl.en-US/Developer Guide/Managing Objects/Object Meta.md#)

