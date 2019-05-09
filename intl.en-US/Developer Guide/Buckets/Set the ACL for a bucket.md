# Set the ACL for a bucket {#concept_fnt_4z1_5db .concept}

You can set the access control list \(ACL\) when creating a bucket, or modify the ACL for a created bucket based on your business needs. Only bucket owners can set or modify the ACL for buckets.

The following table describes the three types of ACLs for buckets.

|ACL|Description|Access control|
|:--|:----------|:-------------|
|public-read-write|The public-read-write permission.|Anyone \(including anonymous users\) can perform read and write operations on the objects in the bucket. **Warning:** All users on the Internet can have access to the objects in the bucket and write data to the bucket. This may leak your bucket data and sharply increase your fees. If anyone maliciously writes illegal information, they may also infringe on your legitimate interests and rights. Therefore, we do recommend that you do not set your bucket ACL to public-read-write except for special needs.

 |
|public-read|The public-read permission.|Only the bucket owner can perform write operations on the objects in the bucket. Other users \(including anonymous users\) can perform only read operations on the objects in the bucket. **Warning:** All users on the Internet can have access to the objects in the bucket. This may leak your bucket data and sharply increase your fees. Therefore, we recommend that you set your bucket ACL to public-read with caution.

 |
|private|The private permission.|Only the bucket owner can perform read and write operations on the objects in the bucket. Other users have no access to the objects in the bucket.|

## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](../../../../reseller.en-US/Console User Guide/Manage buckets/Change bucket ACL.md#)|Web application, which is intuitive and easy to use|
|[ossbrowser](../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#)|Graphical tool, which is easy to operate|
|[ossutil](../../../../reseller.en-US/Tools/ossutil/Bucket-related commands.md#ul_imw_f5s_vdb)|Command-line tool, which delivers good performance|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Manage a bucket.md#table_grn_jjf_2gb)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Bucket.md#section_rvh_l1j_kfb)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Bucket.md#section_ond_15p_kfb)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Bucket.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Bucket.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Manage a bucket.md#)|
|[Node.js SDK](../../../../reseller.en-US/SDK Reference/Node. js/Manage a bucket.md#ul_ict_gqk_lfb)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/Manage buckets.md#ul_px3_pnn_lfb)|

## Reference {#section_skh_zd4_vgb .section}

-   [Overview](reseller.en-US/Developer Guide/Access and control/Overview.md#)
-   [Overview](reseller.en-US/Developer Guide/Access and control/Cross-account authorization/Overview.md#)

