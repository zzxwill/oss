# Create a bucket {#concept_bcz_sbz_5db .concept}

Before uploading any file to OSS, you must create a bucket and specify the attributes of the bucket, including the region, ACL, and other metadata.

## Procedure {#section_azm_ybz_5db .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  If the bucket list is empty, click **Create Bucket** on the left side. If you have created buckets, click **+** in the left-side bucket list or click **Create Bucket** in the upper right corner to create a bucket. The Create Bucket dialog box is displayed, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4740/155313564411303_en-US.png)

3.  Enter a name for the bucket in **Bucket Name**.
    -   The bucket name must comply with the naming conventions.
    -   The bucket name must be unique among all existing OSS buckets.
    -   The name of a bucket cannot be modified after the bucket is created.
    -   For more information about the bucket naming conventions, see [Basic concepts](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#).
4.  In the **Region** drop-down list, select the region where the bucket needs to be created.

    The region of a bucket cannot be modified after the bucket is created. Exercise caution when selecting the region. To access a bucket from an ECS instance through the intranet, select the region to which the ECS instance belongs when creating the bucket. For more information, see [Endpoints](../../../../../reseller.en-US/Developer Guide/Endpoint/Endpoints.md#).

5.  Select one of the following storage classes for **Storage Class** according to your needs.
    -   Standard: provides highly reliable, highly available, and high-performance object storage services that support frequent data accesses.
    -   IA: applies to data that is stored long-term and infrequently accessed. Its unit price is lower than that of the Standard class. Objects of the IA storage class must be stored for a minimum period of 30 days. Fees are incurred if objects of the IA storage class are deleted before they are stored for 30 days. Objects of the IA storage class also have a minimum billable size of 64 KB. Any objects smaller than 64 KB are charged as 64 KB. Furthermore, data retrieval also incurs charges.
    -   Archive: applies to archive data that has cold storage attributes \(that is, the data is to be stored for more than half a year and is only to be accessed sporadically during the storage period\). Note that if you need to convert Archive class data to IA class data, the conversion takes about one minute before the data can be accessed. Examples of objects suitable for the Archive storage class include archive data, medical images, scientific data, and video.
6.  Select an ACL for the bucket in **Access Control List \(ACL\)**.
    -   Private: Only the bucket owner can read data from and write data to objects in the bucket. All other users \(including anonymous users\) cannot perform operations on objects in the bucket without authorization.
    -   Public Read: Only the bucket owner can write data to objects in the bucket. All other users \(including anonymous users\) can only read from objects in the bucket.

        **Warning:** Any user can access the objects in the bucket through the Internet. This may result in data leakage and excessive fees. Therefore, set this ACL with caution.

    -   Public Read/Write: All users \(including anonymous users\) can read from and write to objects in the bucket.

        **Warning:** Any user can access the objects in the bucket and write data to the bucket through the Internet. This may result in data leakage and excessive fees. Your legal rights may be infringed if illegal information is maliciously written to your bucket. Therefore, we recommend you do not set the ACL to public-read-write unless necessary.

7.  Click **OK**.

