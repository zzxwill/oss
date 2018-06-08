# Bucket permission control {#concept_idj_zff_xdb .concept}

OSS provides an Access Control List \(ACL\) for the bucket-level access control. Currently, three access permissions are available for a bucket: Public Read/Write, Public Read, and Private.

|Permission|Name|Access restrictions on visitors|
|:---------|:---|:------------------------------|
|public-read-write|Public read and write| -   Anyone \(including anonymous access\) can read, write, and delete the objects in the bucket.
-   The fees incurred by such operations are borne by the owner of the bucket. Therefore, use this permission carefully.

 |
|public-read|Public read, private write| -   Only the owner of the bucket and the authorized users can perform write and delete operations on the objects in the bucket.
-   Anyone \(including anonymous access\) can read the objects in the bucket.

 |
|private|Private read and write| -   Only the owner of the bucket and the authorized users can perform read, write, and delete operations on the objects in the bucket.
-   Other users cannot access objects in the bucket.

 |

**Note:** 

-   If a new bucket is created without a specified permission, OSS automatically sets a private permission for the bucket.
-   For an existing bucket, only the creator of the bucket can change its permissions by using the Put Bucket ACL interface provided by OSS.

