# Manage Object Meta {#concept_lkf_swy_5db .concept}

Object Meta describes the attributes of objects uploaded to OSS. These attributes are classified into two types: HTTP standard attributes \(HTTP header fields\) and User Meta \(user-defined Object Meta\). You can set Object Meta when uploading or copying objects in various ways.

-   HTTP standard attributes

    |Name|Description|
    |:---|:----------|
    |Cache-Control|The Webpage caching behavior when the object is downloaded.|
    |Content-Disposition|The name of the object during the download.|
    |Content-Encoding|The content encoding format of the object during the download.|
    |Content-Language|The content language encoding when the object is downloaded.|
    |Expires|The time the object expires.|
    |Content-Length|The size of the object.|
    |Content-Type|The type of the object.|
    |Last-Modified|The time the object is last modified.|

-   User Meta

    User Meta allows you to better describe objects. In OSS, all parameters prefixed with x-oss-meta- are considered as User Meta, such as x-oss-meta-location. An object may have multiple similar parameters, but the total size of all User Meta cannot exceed 8 KB. User Meta is returned in the HTTP header of responses to GetObject or HeadObject requests.


## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](../../../../reseller.en-US/Console User Guide/Upload、download and manage objects/Set an HTTP header.md#)|Web application, which is intuitive and easy to use|
|[ossbrowser](../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#)|Graphical tool, which is easy to operate|
|[ossutil](../../../../reseller.en-US/Tools/ossutil/Object-related commands.md#ul_qj4_h1z_5fb)|Command-line tool, which delivers good performance|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Manage objects/Manage Object Meta.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Manage objects/Manage Object Meta.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Manage objects/Manage Object Meta.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Manage objects/Manage Object Meta.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Manage objects/Manage Object Meta.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Manage objects/Manage Object Meta.md#)|
|[Android SDK](../../../../reseller.en-US/SDK Reference/Android/Manage objects/Obtain the metadata of an object.md#)|

## Reference {#section_y5f_qxy_5db .section}

You can also add Object Meta in the following operations:

-   [Simple upload](reseller.en-US/Developer Guide/Upload files/Simple upload.md#)
-   [Multipart upload and resumable upload](reseller.en-US/Developer Guide/Upload files/Multipart upload and resumable upload.md#)
-   [Append upload](reseller.en-US/Developer Guide/Upload files/Append upload.md#)
-   [Copy an object](reseller.en-US/Developer Guide/Manage files/Copy an object.md#)

