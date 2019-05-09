# Obtain the region information of a bucket {#concept_jvv_v1b_5db .concept}

You can use the GetBucketLocation API of OSS to obtain the location information about the region, that is, the data center, where a bucket is located.

**Note:** For more information about the GetBucketLocation API, see [GetBucketLocation](../../../../reseller.en-US/API Reference/Bucket operations/GetBucketLocation.md#).

The returned Location field indicates the region where the bucket is located. For example, if a bucket is located in China \(Hangzhou\), the value of the returned Location field is `oss-cn-hangzhou`. For more information about regions, see [Regions and endpoints](reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).

## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](https://home.console.aliyun.com)|Web application that displays the region of a bucket on the bucket overview page|
|[ossbrowser](../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#)|Graphical tool, which is easy to operate|
|[ossutil](../../../../reseller.en-US/Tools/ossutil/Bucket-related commands.md#ul_imw_f3s_vdb)|Command-line tool, which delivers good performance|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Manage a bucket.md#section_swm_ljf_2gb)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Bucket.md#section_rvh_l1j_kfb)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Bucket.md#section_ond_15p_kfb)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Bucket.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Bucket.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Manage a bucket.md#)|
|[Node.js SDK](../../../../reseller.en-US/SDK Reference/Node. js/Manage a bucket.md#ul_ict_gqk_lfb)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/Manage buckets.md#ul_px3_pnn_lfb)|

