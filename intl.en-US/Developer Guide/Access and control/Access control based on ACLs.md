# Access control based on ACLs {#concept_blw_yqm_2gb .concept}

OSS provides access control lists \(ACLs\) for you to control access permissions. ACLs are access policies that grant bucket and object access permissions to users. You can set the ACL when creating a bucket or uploading an object, and modify the ACL for a created bucket or an uploaded object at any time.

**Note:** For more information about ACL-based OSS APIs, see the following topics:

-   Set the ACL for a bucket: [PutBucketACL](../../../../reseller.en-US/API Reference/Bucket operations/PutBucketACL.md#)
-   Obtain the ACL of a bucket: [GetBucketACL](../../../../reseller.en-US/API Reference/Bucket operations/GetBucketAcl.md#)
-   Set the ACL for an object: [PutObjectACL](../../../../reseller.en-US/API Reference/Object operations/PutObjectACL.md#)
-   Obtain the ACL of an object: [GetObjectACL](../../../../reseller.en-US/API Reference/Object operations/GetObjectACL.md#)

## Bucket ACL {#section_dr5_nvm_2gb .section}

-   Overview

    Bucket ACLs are used to control access to buckets. You can set any of the following three types of ACLs for a bucket: public-read-write, public-read, and private, which are described in the following table.

    |ACL|Description|Access control|
    |:--|:----------|:-------------|
    |public-read-write|The public-read-write permission.|Anyone \(including anonymous users\) can perform read, write, and delete operations on the objects in the bucket. The bucket owner needs to pay the fees incurred by these operations. We recommend that you set this ACL with caution.|
    |public-read|The public-read permission.|Only the bucket owner or authorized users can perform write and delete operations on the objects in the bucket. Other users \(including anonymous users\) can perform only read operations on the objects in the bucket.|
    |private|The private permission.|Only the bucket owner or authorized users can perform read, write, and delete operations on the objects in the bucket. Without authorization, other users have no access to the objects in the bucket.|

-   Operating methods

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


## Object ACL {#section_yft_rvm_2gb .section}

-   Overview

    Object ACLs are used to control access to objects. You can set any of the following four types of ACLs for an object: private, public-read, public-read-write, and default. You can include the `x-oss-object-acl` field in the request header and send a PUT request through the PutObjectACL API to set the ACL for an object. Only the owner of a bucket can perform the PutObjectACL operation on the objects in the bucket.

    The following table describes the four types of ACLs for objects.

    |ACL|Description|Access control|
    |:--|:----------|:-------------|
    |public-read-write|The public-read-write permission.|All users can read data from and write data to the object.|
    |public-read|The public-read permission.|The object owner can read data from and write data to the object. Other users can only read data from the object.|
    |private|The private permission.|The object owner can read data from and write data to the object. Without authorization, other users have no access to the object.|
    |default|The default permission.|The object inherits the ACL from the bucket, that is, the ACL of an object is the same as the ACL of the bucket where the object is stored.|

    **Note:** 

    -   If no ACL is set for an object, the object uses the default ACL, indicating that the object has the same ACL as the bucket where the object is stored.
    -   If an ACL is set for an object, the object ACL takes precedence over the ACL of the bucket where the object is stored. For example, if the ACL of an object is set to public-read, all authenticated and anonymous users can read data from the object regardless of the bucket ACL.
-   Operating methods

    |Operating method|Description|
    |----------------|-----------|
    |[Console](../../../../reseller.en-US/Console User Guide/Upload、download and manage objects/Change object ACL.md#)|Web application, which is intuitive and easy to use|
    |[ossbrowser](../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#)|Graphical tool, which is easy to operate|
    |[ossutil](../../../../reseller.en-US/Tools/ossutil/Object-related commands.md#section_zbv_mfy_bgb)|Command-line tool, which delivers good performance|
    |[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Manage objects/Manage ACL for an object.md#)|SDK demos in various languages|
    |[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Manage objects/Manage ACL for an object.md#)|
    |[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Manage objects/Manage ACL for an object.md#)|
    |[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Manage objects/Manage ACL for an object.md#)|
    |[C SDK](../../../../reseller.en-US/SDK Reference/C/Manage objects/Manage ACL for an object.md#)|
    |[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Manage objects/Manage ACL for an object.md#)|


## Reference {#section_jj1_j33_wgb .section}

For more information about how to allow only specified users to access your objects, see the following topics:

-   -   [Tutorial:Authorize a RAM user under another Alibaba Cloud account by adding a bucket policy](reseller.en-US/Developer Guide/Access and control/Cross-account authorization/Tutorial:Authorize a RAM user under another Alibaba Cloud account by adding a bucket policy.md#)
-   -   
