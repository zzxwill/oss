# Append upload {#concept_ls5_yhb_5db .concept}

By using append upload, you can use the AppendObject API of OSS to directly append content to the appendable objects that are uploaded.

**Note:** For more information about the AppendObject API, see [AppendObject](../../../../reseller.en-US/API Reference/Object operations/AppendObject.md#).

## Scenarios {#section_b3p_zhb_5db .section}

By using [simple upload](reseller.en-US/Developer Guide/Upload files/Simple upload.md#), [form upload](reseller.en-US/Developer Guide/Upload files/Form upload.md#), and [multipart upload and resumable upload](reseller.en-US/Developer Guide/Upload files/Multipart upload and resumable upload.md#), you can only create normal objects that have fixed content after the upload is completed. They can only be read but cannot be modified. To modify the object content, you must upload an object with the same name to overwrite the existing one. This is a major difference between OSS and typical file systems.

Due to this feature, these upload methods are inconvenient in many scenarios, such as video surveillance and video live streaming, because video data is constantly produced in real time. By using these upload methods, you must split a video stream into small parts and then upload them as new objects. In actual use, these methods have obvious defects:

-   The software architecture is complex. You must consider intricate issues such as object splitting.
-   You must reserve space to store metadata, such as the list of created objects. After receiving each request, OSS must repeatedly read the metadata and determine whether to create an object. This puts high pressure on the server. In addition, the client must send each request twice, causing a certain latency.
-   If object parts are small, the latency is low. However, too many objects are hard to manage. If object parts are large, the latency is high.

To simplify development and reduce costs in such scenarios, OSS provides append upload \(through the AppendObject API\), which allows you to directly append content to the end of an object. Objects uploaded by using this method are appendable objects, whereas objects uploaded by using other methods are normal objects. The appended data is instantly readable.

By using append upload, the architecture becomes simple in the preceding scenarios. When video data is produced, it is immediately added to the same object by using append upload. The client only needs to regularly retrieve the object length and compare it with the previous value. If new readable data is found, the client starts a read operation to retrieve the newly uploaded data. This method greatly simplifies the architecture and enhances the scalability.

In addition to video scenarios, append upload can be used to append log data.

## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Upload objects/Append upload.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Upload objects/Append upload.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Upload objects/Append upload.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Upload objects/Append upload.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Upload objects/Append upload.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Upload objects/Append upload.md#)|
|[Android SDK](../../../../reseller.en-US/SDK Reference/Android/Upload objects/Append upload.md#)|
|[iOS SDK](../../../../reseller.en-US/SDK Reference/iOS/Upload objects/Append upload.md#)|

## Upload limits {#section_xkk_b3b_5db .section}

-   Size: The maximum size of an object is 5 GB in this mode.
-   Naming rules:
    -   Object names must be UTF-8 encoded.
    -   Object names must be one byte to 1,023 bytes in length.
    -   Object names cannot start with a forward slash \(/\) or a backslash \(\\\).
-   Object type: You can append data only to objects created by using append upload. Therefore, new data cannot be appended to objects created by using simple upload, form upload, multipart upload, or resumable upload.
-   Subsequent operations: You cannot copy objects created by using append upload. However, you can modify the Object Meta of the objects.

## Upload security and authorization {#section_arr_vbb_5db .section}

To prevent unauthorized third-party users from uploading data to your bucket, OSS provides bucket- and object-level access control. For more information, see [Access control](reseller.en-US/Developer Guide/Access and control/Overview.md#).

To authorize third-party users to upload objects, OSS also provides account authorization. For more information, see [Authorized third-party upload](reseller.en-US/Developer Guide/Upload files/Authorized third-party upload.md#).

## Subsequent operations {#section_emr_xbb_5db .section}

-   After uploading images, you can use [Image Processing](../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#).
-   After uploading audio or video objects, you can use [ApsaraVideo for Media Processing](reseller.en-US/Developer Guide/Cloud data processing.md#).

**Note:** Append upload does not support upload callback.

