# ACL {#concept_blw_yqm_2gb .concept}

OSS provides Access Control Lists \(ACLs\) for permission control. ACLs are access policies that grant bucket and object access permissions to users. You can configure an ACL for a bucket when you create it or for an object when you upload it. You can also modify the ACL for a created bucket or an uploaded object at any time.

## Bucket ACL {#section_dr5_nvm_2gb .section}

Bucket ACLs are configured to control the accesses on buckets. You can configure any of the following three types of ACLs for a bucket: public-read-write, public-read, and private, which are described in the following table.

|Permission|Description|Access restrictions|
|:---------|:----------|:------------------|
|public-read-write|Public read and write|All users \(including anonymous users\) can read, write, and delete objects in the bucket. The fees incurred by these operations are charged to the bucket owner. Exercise caution when you configure this ACL.|
|public-read|Public read and private write|Only the bucket owner can write or delete the objects in the bucket. All other users \(including anonymous users\) can only read objects in the bucket.|
|private|Private read and write|Only the bucket owner can read, write, and delete objects in the bucket. All other users are prohibited from accessing objects in the bucket without authorization.|

You can configure and view bucket ACLs through the console, APIs and SDKs.

-   Console: See [Create a bucket](../../../../../reseller.en-US/Console User Guide/Manage buckets/Create a bucket.md#) and [Change bucket ACL](../../../../../reseller.en-US/Console User Guide/Manage buckets/Change bucket ACL.md#).
-   API: See [PutBucketACL](../../../../../reseller.en-US/API Reference/Bucket operations/Put Bucket ACL.md#) and [GetBucketACL](../../../../../reseller.en-US/API Reference/Bucket operations/GetBucketAcl.md#).
-   SDK: See [Bucket](../../../../../reseller.en-US/SDK Reference/Java/Manage a bucket.md#section_frn_jjf_2gb).

## Object ACL {#section_yft_rvm_2gb .section}

Object ACLs are configured to control the accesses on objects. You can configure any of the following four types of ACLs for an object: private, public-read, public-read-write, and default. You can use the `x-oss-object-acl` header included in the PUT request to perform the PutObjectACL operation. Only the owner of a bucket can perform the PutObjectACL operation on objects in the bucket.

The following table describes the four types of ACLs for objects.

|Permission|Description|Access restrictions|
|:---------|:----------|:------------------|
|public-read-write|Public read and write|All users can read from and write to the object.|
|public-read|Public read and private write|Only the object owner can read from and write to the object. All other users can only read from the object.|
|private|Private read and write|Only the object owner can read from and write to the object. All other users are prohibited from accessing the object without authorization.|
|default|Default ACL|The object inherits the ACL from the bucket, that is, the ACL of an object is the same as the ACL of the bucket where the object is stored.|

**Note:** 

-   If no ACL is configured for an object, the object uses the default ACL, indicating that the object has the same ACL as the bucket where the object is stored.
-   If an ACL is configured for an object, the object ACL takes precedence over the ACL of the bucket where the object is stored. For example, if the ACL of an object is set to public-read, all users can read from the object regardless of the bucket ACL.

You can configure and view object ACLs through the console, APIs and SDKs.

-   Console: See [Create a bucket](../../../../../reseller.en-US/Console User Guide/Manage objects/Upload objects.md#) and [Change object ACL](../../../../../reseller.en-US/Console User Guide/Manage objects/Change object ACL.md#).
-   API: See [PutObjectACL](../../../../../reseller.en-US/API Reference/Object operations/Put Object ACL.md#) and [GetObjectACL](../../../../../reseller.en-US/API Reference/Object operations/GetObjectACL.md#).
-   SDK: See [Manage ACL for an object](../../../../../reseller.en-US/SDK Reference/Java/Manage objects/Manage ACL for an object.md#).

