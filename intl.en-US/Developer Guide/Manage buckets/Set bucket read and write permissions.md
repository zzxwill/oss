# Set bucket read and write permissions {#concept_fnt_4z1_5db .concept}

When creating a bucket, the bucket owner can set the read and write permissions for the bucket using the Access Control List \(ACL\). After a bucket is created, the bucket owner can modify the bucket ACL according to business requirements. Currently, three access permissions are available for a bucket:

|Permission|Access restriction|
|:---------|:-----------------|
|public-read-write|Anyone \(including anonymous users\) can perform read and write operations on the objects stored in the bucket. The fees incurred by these operations are borne by the bucket owner. Use this permission with caution.|
|public-read|Only the bucket owner and authorized users can perform write operations on the objects stored in the bucket. Other people \(including anonymous users\) can perform read operations on the objects.|
|private|Only the bucket owner and authorized users can perform read and write operations on the objects stored in the bucket. Other people cannot access the objects in the bucket without authorization.|

For more information about ACL, see [Access control](intl.en-US/Developer Guide/Access and control/Access control.md#).

## Reference {#section_fmv_tz1_5db .section}

Set bucket ACL:

-   Console: [Set ACL](../../../../intl.en-US/Console User Guide/Manage buckets/Change bucket ACL.md#)
-   SDK: Java SDK-[Set bucket ACL](https://www.alibabacloud.com/help/doc-detail/32012.htm)
-   API: [Put BucketACL](../../../../intl.en-US/API Reference/Bucket operations/Put Bucket ACL.md#)

Get bucket ACL:

-   Console: After logging on to the OSS console, view the ACL on the Basic Settings tab page.
-   SDK: Java SDK-[Get bucket ACL](https://www.alibabacloud.com/help/doc-detail/32012.htm)
-   API: [Get BucketACL](../../../../intl.en-US/API Reference/Bucket operations/GetBucketAcl.md#)

