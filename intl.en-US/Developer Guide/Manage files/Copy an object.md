# Copy an object {#concept_lbg_2zy_5db .concept}

Copying an object is copying the files in the bucket. In certain situations, you want to copy an object to another bucket, without modifying its content. The standard process is to first download the object, and then upload the object to the new bucket. However, because data is identical for both objects, network bandwidth is wasted. To overcome this issue, OSS provides the CopyObject function to copy objects directly within the OSS without the need to transmit large volumes of data between the user and the OSS.

Additionally, because OSS does not support renaming, we recommend that the OSS CopyObject interface is called for renaming an object. This means you can first copy the original data to an object, apply a new name, and then delete the original file. To only modify an object’s Object Meta \(object metadata\), you can also call the CopyObject interface and set the source address and destination address to the same value. In this way, the OSS only updates the Object Meta. For more information about Object Meta, see [Object Meta](reseller.en-US/Developer Guide/Manage files/Object Meta.md#).

Before copying an object, note the following precautions:

-   You must have permissions to operate the source object. Otherwise the operation fails.
-   Data cannot be copied across regions.  For example, an object in a Hangzhou bucket may not be copied to a Qingdao bucket.
-   Objects up to 1 GB are supported.
-   Appended objects cannot be copied.

Reference:

-   API: [CopyObject](../../../../reseller.en-US/API Reference/Object operations/CopyObject.md#)

## Copy large objects {#section_dn5_lzy_5db .section}

The OSS supports the function of copying large files similar to [Multipart upload](reseller.en-US/Developer Guide/Upload files/Multipart upload.md#).

The only difference is that the process [UploadPart](reseller.en-US/Developer Guide/Upload files/Multipart upload.md#) is replaced by the process [UploadPartCopy](../../../../reseller.en-US/API Reference/Multipart upload operations/UploadPartCopy.md#).

The syntax of [UploadPartCopy](../../../../reseller.en-US/API Reference/Multipart upload operations/UploadPartCopy.md#) is similar to that of [UploadPart](../../../../reseller.en-US/API Reference/Multipart upload operations/UploadPart.md#). However, instead of being directly uploaded from the HTTP request, data is retrieved from the source object.

Reference:

-   API: [UploadPartCopy](../../../../reseller.en-US/API Reference/Multipart upload operations/UploadPartCopy.md#)

