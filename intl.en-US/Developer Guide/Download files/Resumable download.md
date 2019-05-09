# Resumable download {#concept_zjn_31c_5db .concept}

OSS allows you to download data from a specified position of an object. When downloading a large object, you can split it into multiple parts and download them at different points of time. If a download is paused or interrupted, you can also resume it at the paused or interrupted position.

Similar to simple upload, you must have the read permission on the object. You can set the Range parameter to enable resumable download. We recommend that you use this feature to download large objects. For more information about the Range parameter, see the relevant RFC. If the Range parameter is specified in the request header, the response contains the length of the entire object and the range returned in this response. For example, Content-Range: bytes 0–9/44 indicates that the length of the entire object is 44 bytes, and the range returned this time is the first 10 bytes. If the specified Range parameter value is invalid, the entire object is transmitted. The response does not include Content-Range, but returns HTTP status code 206.

## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Download objects/Resumable download.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Download objects/Resumable download.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Download objects/Resumable download.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Download objects/Resumable download.md#)|
|[Android SDK](../../../../reseller.en-US/SDK Reference/Android/Download objects/Resumable download.md#)|
|[iOS SDK](../../../../reseller.en-US/SDK Reference/iOS/Download objects/Resumable download.md#)|

## Download security and authorization {#section_arr_vbb_5db .section}

-   To prevent unauthorized third-party users from downloading data from your bucket, OSS provides bucket- and object-level access control. For more information, see [Access control](reseller.en-US/Developer Guide/Access and control/Overview.md#).
-   For more information about how to authorize a third-party user to download objects from a private bucket, see [Authorized third-party download](reseller.en-US/Developer Guide/Download files/Authorized third-party download.md#).

