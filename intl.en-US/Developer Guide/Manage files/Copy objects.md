# Copy objects {#concept_lbg_2zy_5db .concept}

You can copy objects from a bucket to another bucket without modifying the object content.

Previously, to copy an object, you need to download it and then upload it to the destination bucket. This method wastes network bandwidth resources. Therefore, OSS provides the CopyObject API to copy objects within OSS. You do not need to transmit large amounts of data from and to OSS.

In addition, OSS does not allow you to rename objects. We recommend that you call the CopyObject API to first copy the data of an object to another object with a new name, and then delete the original object. To modify only the Object Meta of an object, you can also call the CopyObject API and set the source address and the destination address to the same value. In this way, OSS only updates Object Meta. For more information about Object Meta, see [Manage Object Meta](reseller.en-US/Developer Guide/Manage files/Manage Object Meta.md#).

## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[ossbrowser](../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#)|Graphical tool, which is easy to operate **Note:** You can only use ossbrowser to copy objects smaller than 5 GB.

 |
|[ossutil](../../../../reseller.en-US/Tools/ossutil/Object-related commands.md#)|Command-line tool, which delivers good performance|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Manage objects/Copy objects.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Manage objects/Copy objects.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Manage objects/Copy objects.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Manage objects/Copy objects.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/. NET/Manage objects/Copy objects.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Manage objects/Copy objects.md#)|
| |
|[Node.js SDK](../../../../reseller.en-US//Manage objects.md#)|
|[Browser.js SDK](../../../../reseller.en-US/SDK Reference/Browser.js/Manage objects.md#)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/Manage objects.md#)|

## Notes {#section_wfp_42c_wgb .section}

When copying objects, you need to note the following items:

-   You must have operation permissions on the source object. Otherwise, the operation may fail.
-   You are not allowed to copy data across regions. For example, you are not allowed to copy an object in a bucket in China \(Hangzhou\) to a bucket in China \(Qingdao\).
-   You are not allowed to copy objects created by using append upload.
-   You can use the [CopyObject](../../../../reseller.en-US/API Reference/Object operations/CopyObject.md#) API to copy objects smaller than 1 GB in simple copy mode.
-   You can use the [UploadPartCopy](../../../../reseller.en-US/API Reference/Multipart upload operations/UploadPartCopy.md#) API to copy objects larger than 1 GB in multipart copy mode.

