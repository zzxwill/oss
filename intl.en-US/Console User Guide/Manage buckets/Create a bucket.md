# Create a bucket {#concept_bcz_sbz_5db .concept}

Before uploading any files to the OSS, you must create a bucket to store files. You can specify the attributes of the bucket, including the region, access permission, and other metadata.

## Procedure {#section_azm_ybz_5db .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  Click **Create Bucket** to open the **Create Bucket** dialog box.
3.  In the **Bucket Name** field, enter the bucket name.
    -   The bucket name must comply with the naming conventions.
    -   The bucket name must be unique across all existing bucket names in OSS.
    -   The bucket name cannot be changed after the bucket is created.
    -   For more information about bucket naming, see [Basic concepts](https://www.alibabacloud.com/help/doc-detail/31827.html).
4.  In the **Region** drop-down box, select the data center of the bucket.

    The region cannot be changed after the subscription. To access the OSS over the intranet of the ECS, select the same region with your ECS instance. For more information, see [Endpoints](https://www.alibabacloud.com/help/doc-detail/31834.html).

5.  For **Storage Class**, select the storage type as needed.
    -   Standard storage: provides highly reliable, highly available, and high-performance object storage services that support frequent data accesses.
    -   Infrequent access: suitable for data that is stored for a long term and infrequently accessed. Its unit price is lower than that of the standard type. This storage class requires a minimum storage duration for the files. Charges are incurred if you delete files that are stored for less than 30 days. This storage class requires a minimum billable size for files. Files smaller than 128 KB are charged for 128 KB and data retrieval may cause a certain cost.
    -   Archive storage: suitable for storing archival data that requires long-term persistence \(more than half a year\). The data is infrequently accessed during the storage period and restoring the data to a readable state may take one minute. It is suitable for storing archival data, medical images, scientific materials, and video footages for long-term persistence.
6.  For **ACL**, select the expected permission for the bucket.
    -   Private: Only the owner of the bucket and the authorized users can perform read, write, and delete operations on the objects in the bucket. Other users cannot access objects in the bucket.
    -   Public Read: Only the owner of the bucket and the authorized users can perform write and delete operations on the objects in the bucket. Anyone \(including anonymous access\) can read the objects in the bucket.
    -   Public Read/Write: Anyone \(including anonymous access\) can read, write, and delete the objects in the bucket. 

        **Note:** The fees incurred by such operations are borne by the owner of the bucket. Use this permission with caution.

7.  Click **OK**.

