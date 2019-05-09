# Create a bucket {#concept_ntj_wx1_5db .concept}

Before uploading objects to OSS, you must use the PutBucket API of OSS to create a bucket to store objects. You can configure various attributes for a bucket, including the region, access permissions, and other metadata.

**Note:** For more information about the PutBucket API, see [PutBucket](../../../../reseller.en-US/API Reference/Bucket operations/PutBucket.md#).

## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](../../../../reseller.en-US/Console User Guide/Manage buckets/Create a bucket.md#)|Web application, which is intuitive and easy to use|
|[ossbrowser](../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#)|Graphical tool, which is easy to operate|
|[ossutil](../../../../reseller.en-US/Tools/ossutil/Bucket-related commands.md#ul_nxp_chs_vdb)|Command-line tool, which delivers good performance|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Manage a bucket.md#section_zgm_l1b_kfb)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Bucket.md#section_rvh_l1j_kfb)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Bucket.md#section_ond_15p_kfb)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Bucket.md#section_zgm_l1b_kfb)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Bucket.md#section_ogg_55x_kfb)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Manage a bucket.md#section_s5m_v12_lfb)|
|[Android SDK](../../../../reseller.en-US/SDK Reference/Android/Manage buckets.md#section_lwh_f2j_lfb)|
|[iOS SDK](../../../../reseller.en-US/SDK Reference/iOS/Manage a bucket.md#section_lwh_f2j_lfb)|
|[Node.js SDK](../../../../reseller.en-US/SDK Reference/Node. js/Manage a bucket.md#)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/Manage buckets.md#)|

## Limits {#section_nkx_kdg_tgb .section}

-   You can create a maximum of 30 buckets.
-   The name of each bucket must be globally unique. To create a bucket, you must select a unique bucket name.
-   Each bucket name must comply with the bucket naming rules.
-   After a bucket is created, you cannot modify its name or region.

## Access control {#section_ybt_lz1_5db .section}

You can set the access control list \(ACL\) when creating a bucket, or modify the ACL for a created bucket. If you do not set the ACL, it is set to private by default. For more information, see [Set the ACL for a bucket](reseller.en-US/Developer Guide/Buckets/Set bucket permissions (ACL).md#).

## Reference {#section_wwt_tjg_tgb .section}

-   [Simple upload](reseller.en-US/Developer Guide/Upload files/Simple upload.md#)
-   [Simple download](reseller.en-US/Developer Guide/Download files/Simple download.md#)
-   [Delete a bucket](reseller.en-US/Developer Guide/Buckets/Delete a bucket.md#)
-   [Overview](reseller.en-US/Developer Guide/Access and control/Overview.md#)

