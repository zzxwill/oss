# Bucket access control {#concept_idj_zff_xdb .concept}

OSS provides an Access Control List \(ACL\) for bucket-level access control. Currently, three ACLs are available for a bucket: public-read-write, public-read, and private.

|ACL|Permission|Description|
|:--|:---------|:----------|
|public-read-write|Public read and write|Any user \(including anonymous users\) can perform read/write operations, and delete operations on objects in the bucket.**Warning:** We recommend you that do not set the ACL of a bucket to public-read-write to avoid incurring excessive fees or having your account suspended due to malicious or illegal activities of another user.

|
|public-read|Public read and private write|Only the owner of the bucket can perform write operations on objects in the bucket. All other users \(including anonymous users\) can only perform read operations on objects in the bucket.**Warning:** We recommend that you exercise caution when setting this ACL because it authorizes any user to perform read operations on objects in the bucket through the Internet, which may incur excessive fees.

|
|private|Private read and write|Only the owner of the bucket can perform read/write operations on the objects in the bucket. Other users cannot access the objects.|

**Note:** 

-   If you do not set an ACL for a bucket when you create it, its ACL is set to private automatically.
-   If the ACL rule of the bucket is set to private, only authorized users can access and operate on objects in the bucket. For more information about access control, see [Access control](../../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#).
-   Only the creator of an existing bucket can modify the ACL for the bucket by using the PutBucketACL API.

