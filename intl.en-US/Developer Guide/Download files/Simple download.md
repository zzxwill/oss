# Simple download {#concept_irq_zzb_5db .concept}

By using simple download, you can send an HTTP GET request to download an uploaded object through the GetObject API of OSS.

**Note:** 

-   For more information about the GetObject API, see [GetObject](../../../../reseller.en-US/API Reference/Object operations/GetObject.md#).
-   For more information about the rules for generating object URLs, see [OSS request process](reseller.en-US/Developer Guide/Signature/OSS request process.md#).
-   For more information about how to use a custom domain to access an object, see [Bind a custom domain](reseller.en-US/Developer Guide/Buckets/Bind a custom domain.md#).

## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](../../../../reseller.en-US/Console User Guide/Upload、download and manage objects/Download an object.md#)|Web application, which is intuitive and easy to use|
|[ossbrowser](../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#)|Graphical tool, which is easy to operate|
|[ossutil](../../../../reseller.en-US/Tools/ossutil/Object-related commands.md#)|Command-line tool, which delivers good performance|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Download objects/Download to your local file.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Download objects/Download to your local file.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Download objects/Download to your local file.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Download objects/Download to your local file.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Download objects/Download an object to a local file.md#)|
| |
| |
|[Node.js SDK](../../../../reseller.en-US//Download objects.md#)|
|[Browser.js SDK](../../../../reseller.en-US/SDK Reference/Browser.js/Download objects.md#)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/Download objects.md#)|

## Download security and authorization {#section_arr_vbb_5db .section}

-   To prevent unauthorized third-party users from downloading data from your bucket, OSS provides bucket- and object-level access control. For more information, see [Access control](reseller.en-US/Developer Guide/Access and control/Overview.md#).
-   For more information about how to authorize a third-party user to download objects from a private bucket, see [Authorized third-party download](reseller.en-US/Developer Guide/Download files/Authorized third-party download.md#).

## Reference {#section_ynv_c1c_5db .section}

For more information about how to use resumable download, see [Resumable download](reseller.en-US/Developer Guide/Download files/Multipart download.md#).

