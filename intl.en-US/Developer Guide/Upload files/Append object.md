# Append object {#concept_ls5_yhb_5db .concept}

## Applicable scenarios {#section_b3p_zhb_5db .section}

The [Simple Upload](intl.en-US/Developer Guide/Upload files/Simple upload.md#), [Form Upload](intl.en-US/Developer Guide/Upload files/Form upload.md#), and [Multipart Upload](intl.en-US/Developer Guide/Upload files/Multipart upload.md#) methods create normal-type objects which have fixed content after the upload is finished. They can only be read, but cannot be modified.  If the object content changes, the user must upload an object of the same name to overwrite the content. This is a major difference between OSS and file systems.

This feature makes many application scenarios inconvenient, such as video monitoring and live video broadcast, since video data is constantly produced in real time.  Using other upload methods, users must slice the video stream into small pieces and then upload them as new objects.  In actual use, these methods have obvious defects:

-   The software architecture is quite complex and users must consider intricate issues such as file fragments.
-   Storage space is required for metadata, e.g. the list of generated objects. Thus, each request must read the metadata to judge if any new object has been generated. This puts a high level of access pressure on the server. In addition, each client request must be transmitted twice, causing a certain amount of delay.
-   If the object parts are small, the delay is quite short. However this complicates the management of most objects.  If the object parts are large, the data suffers a substantial delay.

To development and reduce costs in such a scenario, OSS provides the append  object method, which allows users to directly append content to the end of an object.   This method is used to operate on Appendable   objects. The objects uploaded by other methods are Normal objects.   The data appended is instantly readable.

With append object, the previous scenario becomes simple.  When video data is produced, they can be immediately added to the same object through the append object method. The client only needs to regularly retrieve the object length and compare it with the previous value. If new readable data is found, this triggers a read operation to retrieve the newly uploaded data segments.  This method greatly simplifies the architecture and enhances the scalability of applications.

In addition to video scenarios, the append object method can also be used to append log data.

## Upload restrictions {#section_xkk_b3b_5db .section}

-   Size limit: The maximum object size is 5 GB in this mode.
-   Naming restrictions
    -   It uses UTF-8 encoding.
    -   The length must be 1-1,023 bytes.
    -   It cannot start with “/“ or “\\”.
-   File type: Only files created through append object can be appended with new data.  Therefore, new data cannot be appended to files created through simple upload, form upload, or multipart upload.
-   Subsequent operation restrictions: No files created through append object can be copied, but you can modify the meta-information for the file itself.

## Upload security and authorization {#section_lyr_d3b_5db .section}

To prevent unauthorized third parties from uploading objects to the developer’s bucket, OSS provides bucket- and object-level access permission control. For more information, see [Access Control](intl.en-US/Developer Guide/Access and control/Access control.md#). In addition to bucket-level and object-level access permissions, OSS also provides account-level authorization to authorize third-party uploads. For more information, see [Authorized Third-party Upload for Upload Security](intl.en-US/Developer Guide/Upload files/Authorized third-party upload.md#)。

## Post-upload Operations {#section_ssw_23b_5db .section}

To process uploaded images, users can use [Cloud Processing for Uploaded Images](intl.en-US/Developer Guide/Image Processing.md#). For audio/video file format conversion, users can use [Media Transcoding](intl.en-US/Developer Guide/Cloud data processing.md#).

## Reference for using the function {#section_s25_f3b_5db .section}

-   API：[Append Object](../intl.en-US/API Reference/Object operations/AppendObject.md#)
-   SDK：Java SDK-[Append Object Example](https://www.alibabacloud.com/help/doc-detail/32013.htm)

**Note:** Append object method does not support upload callback.

## Best practices {#section_p5k_j3b_5db .section}

-   [RAM and STS User Guide](../intl.en-US/Best Practices/Access control/Overview.md#)

## Reference links {#section_d2j_k3b_5db .section}

-   [Downloading Uploaded Files](intl.en-US/Developer Guide/Download Files/Simple download.md#)
-   [Cloud Processing for Uploaded Images](intl.en-US/Developer Guide/Image Processing.md#)
-   [Cloud Processing for Uploaded Audio and Video Files](intl.en-US/Developer Guide/Cloud data processing.md#)
-   [Access Control for Upload Security](intl.en-US/Developer Guide/Access and control/Access control.md#)
-   [Authorized Third-party Upload for Upload Security](intl.en-US/Developer Guide/Upload files/Authorized third-party upload.md#)

