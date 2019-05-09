# Simple upload {#concept_bws_3bb_5db .concept}

By using simple upload, you can use the PutObject API of OSS to upload single objects. Simple upload is applicable to scenarios where you can send an HTTP request to complete an upload, for example, to upload an object that is smaller than 5 GB.

**Note:** 

-   For more information about the PutObject API, see [PutObject](../../../../reseller.en-US/API Reference/Object operations/PutObject.md#).
-   To upload an object that is larger than 5 GB, you can use [Resumable upload](reseller.en-US/Developer Guide/Upload files/Multipart upload.md#).

## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](../../../../reseller.en-US/Console User Guide/Upload、download and manage objects/Upload objects.md#)|Web application, which is intuitive and easy to use|
|[ossbrowser](../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#)|Graphical tool, which is easy to operate|
|[ossutil](../../../../reseller.en-US/Tools/ossutil/Object-related commands.md#)|Command-line tool, which delivers good performance|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Upload objects/Simple upload.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Upload objects/Simple upload.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Upload objects/Simple upload.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/PHP/Upload objects/Simple upload.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Upload objects/Simple upload.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Upload objects/Simple upload.md#)|
|[Android SDK](../../../../reseller.en-US/SDK Reference/Android/Upload objects/Overview.md#)|
|[iOS SDK](../../../../reseller.en-US/SDK Reference/iOS/Upload objects/Overview.md#)|
|[Node.js SDK](../../../../reseller.en-US//Upload objects.md#)|
|[Browser.js SDK](../../../../reseller.en-US/SDK Reference/Browser.js/Upload objects.md#)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/Upload objects.md#)|

## Upload limits {#section_ngq_qbb_5db .section}

-   Size: The maximum size of an object is 5 GB in this mode.
-   Naming rules:
    -   Object names must be UTF-8 encoded.
    -   Object names must be one byte to 1,023 bytes in length.
    -   Object names cannot start with a forward slash \(/\) or a backslash \(\\\).

## Object Meta setting {#section_mr2_nbb_5db .section}

When using simple upload, you can set Object Meta to describe an object, for example, Content-Type and other standard HTTP header fields. You can also set user-defined information. For more information, see [Manage Object Meta](reseller.en-US/Developer Guide/Manage files/Object Meta.md#).

## Upload security and authorization {#section_arr_vbb_5db .section}

To prevent unauthorized third-party users from uploading data to your bucket, OSS provides bucket- and object-level access control. For more information, see [Access control](reseller.en-US/Developer Guide/Access and control/Overview.md#).

To authorize third-party users to upload objects, OSS also provides account authorization. For more information, see [Authorized third-party upload](reseller.en-US/Developer Guide/Upload files/Authorized third-party upload.md#).

## Subsequent operations {#section_emr_xbb_5db .section}

-   After uploading objects to OSS, you can use [upload callback](reseller.en-US/Developer Guide/Upload files/Upload callback.md#) to submit a callback request to the specified application server and perform subsequent operations.
-   After uploading images, you can use [Image Processing](../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#).
-   After uploading audio or video objects, you can use [ApsaraVideo for Media Processing](reseller.en-US/Developer Guide/Cloud data processing.md#).

