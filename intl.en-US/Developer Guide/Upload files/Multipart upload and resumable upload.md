# Multipart upload and resumable upload {#concept_wzs_2gb_5db .concept}

By using multipart upload and resumable upload provided by Alibaba Cloud OSS, you can split an object into multiple data blocks \(parts\) and upload them separately. After uploading all the object parts, you can call an API to combine them into an object.

## Scenarios {#section_imy_gbg_mgb .section}

When you use simple upload \(through the PutObject API\) to upload a large object to OSS, the upload may fail due to a network error. In the second upload attempt, you must upload the object from the beginning. In this case, you can use multipart upload to resume upload from the last uploaded part.

Compared with other upload methods, multipart upload is applicable to the following scenarios:

-   Poor network connectivity: If the upload of one part fails on a mobile phone, you can re-upload only the failed part but not all parts.
-   Resumable upload required: An upload in progress can be paused and resumed at any time.
-   Upload acceleration: When the object to be uploaded to OSS is large, multiple parts can be uploaded concurrently to speed up the process.
-   Streaming upload: Objects of unknown sizes can be uploaded at any time. This scenario is common in industry applications such as video surveillance.

## Multipart upload {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[ossutil](../../../../reseller.en-US/Tools/ossutil/Multipart-related commands.md#)|Command-line tool, which delivers good performance|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Upload objects/Multipart upload.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Upload objects/Multipart upload.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Upload objects/Multipart upload.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Upload objects/Multipart upload.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Upload objects/Multipart upload.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Upload objects/Multipart upload.md#)|

## Resumable upload {#section_k5c_lgp_mgb .section}

If the system crashes during a multipart upload, you can resume the upload by using the [ListMultipartUploads](../../../../reseller.en-US/API Reference/Multipart upload operations/ListMultipartUploads.md#) and [ListParts](../../../../reseller.en-US/API Reference/Multipart upload operations/ListParts.md#) APIs to retrieve all multipart upload tasks on an object and list the uploaded parts in each task. This allows an upload task to be resumed from the last uploaded part. The same principles apply if you pause and then resume an upload.

|Operating method|Description|
|----------------|-----------|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Upload objects/Resumable upload.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Upload objects/Resumable upload.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Upload objects/Resumable upload.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Upload objects/Resumable upload.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Upload objects/Resumable upload.md#)|
|[Android SDK](../../../../reseller.en-US/SDK Reference/Android/Upload objects/Resumable upload.md#)|
|[iOS SDK](../../../../reseller.en-US/SDK Reference/iOS/Upload objects/Resumable upload.md#)|

## Upload limits {#section_cxd_mhb_5db .section}

-   Size: The maximum size of an object is determined by the size of parts. Multipart upload supports a maximum of 10,000 parts. Each part must be at least 100 KB \(except for the last part, which may be smaller\) and no more than 5 GB. Therefore, the object size cannot exceed 48.8 TB.
-   Naming rules
    -   Object names must be UTF-8 encoded.
    -   Object names must be one byte to 1,023 bytes in length.
    -   Object names cannot start with a forward slash \(/\) or a backslash \(\\\).

## Multipart upload process {#section_mzy_xgb_5db .section}

The multipart upload process is as follows:

1.  Split the object to be uploaded into multiple parts.
2.  Initialize a multipart upload task \(through the [InitiateMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/InitiateMultipartUpload.md#) API\).
3.  Upload the parts one by one or concurrently \(through the [UploadPart](../../../../reseller.en-US/API Reference/Multipart upload operations/UploadPart.md#) API\).
4.  Complete the upload \(through the [CompleteMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#) API\).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4363/15573928601058_en-US.png)

During the multipart upload process, you need to note the following items:

-   Each part except the last one cannot be smaller than 100 KB. Otherwise, you may fail to call the [CompleteMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#) API.
-   After the object to be uploaded is split into parts, they are sorted by partNumber specified during the upload. However, because upload in sequence is not required, the parts can be uploaded concurrently.

    Due to network conditions and the device load, the upload does not necessarily speed up when more parts are uploaded concurrently. We recommend that you increase the part size in good network conditions, otherwise decrease the part size.

-   By default, when all parts are uploaded but you have not called the [CompleteMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#) API, the uploaded parts are not deleted automatically. You can call the [AbortMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/AbortMultipartUpload.md#) API to terminate the upload and delete the parts that occupy the storage space. For more information about how to automatically delete the uploaded parts, see [Manage object lifecycle](reseller.en-US//Manage lifecycle rules.md#).

## Upload security and authorization {#section_arr_vbb_5db .section}

To prevent unauthorized third-party users from uploading data to your bucket, OSS provides bucket- and object-level access control. For more information, see [Access control](reseller.en-US/Developer Guide/Access and control/Overview.md#).

To authorize third-party users to upload objects, OSS also provides account authorization. For more information, see [Authorized third-party upload](reseller.en-US/Developer Guide/Upload files/Authorized third-party upload.md#).

## Subsequent operations {#section_emr_xbb_5db .section}

-   After uploading objects to OSS, you can use [upload callback](reseller.en-US/Developer Guide/Upload files/Upload callback.md#) to submit a callback request to the specified application server and perform subsequent operations.
-   After uploading images, you can use [Image Processing](../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#).
-   After uploading audio or video objects, you can use [ApsaraVideo for Media Processing](reseller.en-US/Developer Guide/Cloud data processing.md#).

## API reference {#section_p4k_qhb_5db .section}

-   Multipart upload APIs:
    -   [MultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/Introduction.md#)
    -   [InitiateMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/InitiateMultipartUpload.md#)
    -   [UploadPart](../../../../reseller.en-US/API Reference/Multipart upload operations/UploadPart.md#)
    -   [UploadPartCopy](../../../../reseller.en-US/API Reference/Multipart upload operations/UploadPartCopy.md#)
    -   [CompleteMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#)
    -   [AbortMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/AbortMultipartUpload.md#)
    -   [ListMultipartUploads](../../../../reseller.en-US/API Reference/Multipart upload operations/ListMultipartUploads.md#)
    -   [ListParts](../../../../reseller.en-US/API Reference/Multipart upload operations/ListParts.md#)

