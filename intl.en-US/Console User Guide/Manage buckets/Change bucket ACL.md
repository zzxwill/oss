# Change bucket ACL {#concept_d2d_3gz_5db .concept}

OSS provides an Access Control List \(ACL\) for permission control. You can configure an ACL when creating a bucket and change the ACL after the bucket is created.  If you do not configure an ACL for a bucket,  the default ACL of the bucket is Private.

OSS ACL provides bucket-level access control. Currently, three access permissions are available for a bucket:

-   Private: Only the owner of the bucket can perform read/write operations on the objects in the bucket. Other users cannot access the files.
-   Public Read: Only the owner of the bucket can perform write operations on the objects in the bucket, while anyone \(including anonymous users\) can perform read operations on the objects.
-   Public Read/Write: Anyone \(including anonymous users\) can perform read and write operations on the objects in the bucket. The fees incurred by these operations are borne by the owner of the bucket. Configure this permission with caution.

This section describes how to change permission access control at the bucket level.

## Procedure {#section_vdr_ngz_5db .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/) .
2.  On the bucket list on the left, click the target bucket to open the overview page of the bucket.
3.  Click the **Basic Settings** tab and find **ACL** area.
4.  Click **Setting** and change the bucket ACL.
5.  Click **Save** .

