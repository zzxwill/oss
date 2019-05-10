# Delete objects {#concept_g42_bhd_5db .concept}

You can delete objects that are uploaded to OSS buckets.

OSS allows you to perform the following deletion operations:

-   Single deletion: You can delete a specific object.
-   Batch deletion: You can delete up to 1,000 objects at a time.
-   Automatic deletion: We recommend that you [manage object lifecycle](reseller.en-US//Manage lifecycle rules.md#) to enable automatic object deletion if you need to delete a large number of objects based on certain rules, for example, you need to regularly delete objects that were created certain days ago or empty a bucket. After you set lifecycle management rules, OSS automatically deletes expired objects based on the rules. This greatly reduces the number of deletion requests that you send and improves the deletion efficiency.

## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](../../../../reseller.en-US/Console User Guide/Upload、download and manage objects/Delete an object.md#)|Web application, which is intuitive and easy to use|
|[ossbrowser](../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#)|Graphical tool, which is easy to operate|
|[ossutil](../../../../reseller.en-US/Tools/ossutil/Multipart-related commands.md#ul_xtp_zzs_vdb)|Command-line tool, which delivers good performance|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Manage objects/Delete objects.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Manage objects/Delete objects.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Manage objects/Delete objects.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Manage objects/Delete objects.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Manage objects/Delete objects.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Manage objects/Delete objects.md#)|
| |
|[Node.js SDK](../../../../reseller.en-US//Manage objects.md#)|
|[Browser.js SDK](../../../../reseller.en-US/SDK Reference/Browser.js/Manage objects.md#)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/Manage objects.md#)|

