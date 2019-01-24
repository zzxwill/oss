# Configure cross-region replication {#concept_h3r_shf_vdb .concept}

Cross-region replication is used to automatically and asynchronously copy objects across buckets in different regions. Any changes \(creation, replacement, and deletion\) to objects in the source bucket will be synchronized to the target bucket.

**Note:** Currently, the cross-region replication feature is only supported between different regions in Mainland China.

## Procedure {#section_a4q_5hf_vdb .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the name of the bucket that you want to configure cross-region replication.
3.  Click the **Basic Settings** tab and find the **Cross-Region Replication \(CRR\)** region.
4.  Click **Enable** to open the Cross-Region Replication \(CRR\) dialog box.
5.  Select the region and the name of the target bucket.

    **Note:** 

    -   The source bucket and target bucket in synchronization must be in different regions.
    -   Two buckets with cross-region replication enabled cannot be synchronized to other buckets.
6.  Select from the following two options for **Applied To**.
    -   **All Files in Source Bucket**: Synchronizes all objects in the bucket to the target bucket.
    -   **Files with Specified Prefix**: Synchronizes the objects with specified prefixes in the bucket to the target bucket. For example, you have a folder named management in the root directory of a bucket and a folder named abc under management. If you want to synchronize objects in the abc folder, add the management/abc as the prefix. You can add a maximum of five prefixes.
7.  Select from the following options for **Operations**:
    -   **Add/Delete/Change**: Synchronizes all data in the bucket \(including the add, change, and delete operations\) to the target bucket.
    -   **Add/Change**: Synchronizes only added or changed data in the bucket to the target bucket.
8.  Select whether to **Replicate Historical Data**.

    **Note:** If you enable Replicate Historical Data, objects replicated from the source bucket may overwrite the objects with the same names in the target bucket. Therefore, ensure the data in the source and target buckets is consistent before replication.

9.  Click **OK**.

**Note:** 

-   After the configuration is complete, it may take three to five minutes for cross-region replication to be enabled. Information related to synchronization is displayed after the source bucket is synchronized.
-   In cross-region replication, data is replicated asynchronously. Therefore, it usually takes several minutes or hours to replicate data to the target bucket according to the data size.

