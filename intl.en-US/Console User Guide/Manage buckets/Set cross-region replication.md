# Set cross-region replication {#concept_h3r_shf_vdb .concept}

Currently, cross-region replication supports the synchronization of buckets with different names. If you have two buckets belonging to different regions, you can enable the cross-region replication feature in the console to synchronize data from the source bucket to the target bucket.

**Note:** 

Currently, the cross-region replication feature is only supported between different regions in Mainland China and between Eastern and Western United States. It is not supported in other regions currently.

## Procedure {#section_a4q_5hf_vdb .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the name of the target bucket to open the overview page of the bucket.
3.  Click the **Basic Settings** tab, and locate the **Cross-region Replication**.
4.  Click **Enable Synchronization** to open the Cross-region Replication dialog box.
5.  Select the region and name of the target bucket.

    **Note:** 

    -   The two buckets for data synchronization must belong to different regions. Data synchronization is unavailable between buckets in the same region.
    -   The two buckets with cross-region replication enabled cannot have a synchronization relationship with any other buckets.
6.  Select the **Data Synchronization Object**.
    -   **Synchronize all files**: Synchronize all the files in the bucket to the target bucket.
    -   **Synchronize files with specific prefixes**: Synchronize files with specific prefixes in the bucket to the target bucket. Up to 5 prefixes can be added.
7.  Select the **Data Synchronization Policy**.
    -   **Full synchronization \(add/delete/change\)**: Synchronize all the data in the bucket to the target bucket, including added, changed, and deleted data.
    -   **Write synchronization \(add/modify\)**: Synchronize only the added and changed data in the bucket to the target bucket.
8.  Choose whether to **Synchronize Historical Data**.

    **Note:** During the synchronization of historical data, objects replicated from the source bucket may overwrite the objects with the same names in the target bucket. Therefore, check the data consistency before replication.

9.  Click **OK**.

**Note:** 

-   After the configuration is complete, it may take three to five minutes for cross-region replication to be enabled. Synchronization-related information is displayed after the bucket synchronization. 
-   Since the cross-region replication of a bucket is asynchronous, it usually takes several minutes or hours to copy data to the target bucket, depending on the size of data.

