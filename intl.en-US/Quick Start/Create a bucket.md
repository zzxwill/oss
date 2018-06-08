# Create a bucket {#task_u3p_3n4_tdb .task}

After activating Alibaba Cloud OSS, you create a bucket in the OSS console to store objects.

1.   Log on to the [OSS console](https://oss.console.aliyun.com/). 
2.   Click **Create Bucket**  to open the Create Bucket dialog box.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4333/933_en-US.png)

 
3.   In the Bucket Name text box, enter a bucket name. 
    -   The bucket name must comply with the naming rules stated and
    -   must be unique among all existing buckets in Alibaba Cloud OSS.
    -   The bucket name cannot be changed
    -   after being created.
4.   In the Region drop-down box, select the data center of the bucket.  

    The region cannot be changed after the bucket is created. To access OSS through the ECS intranet,  select the same region as your ECS.

5.   In the Storage Class drop-down box, select a storage class for the bucket. 
    -   Standard storage: provides highly reliable, highly available, and high-performance object storage services that support frequent data accesses.
    -   Infrequent access: suitable for data that is stored for a long term and infrequently accessed. Its unit price is lower than that of the standard type. This storage class requires a minimum storage duration for the files. Charges are incurred if you delete files that are stored for less than 30 days. This storage class requires a minimum billable size for files. Files smaller than 128 KB are charged for 128 KB and data retrieval may cause a certain cost.
    -   Archive storage: suitable for storing archival data that requires long-term persistence \(more than half a year\). The data is infrequently accessed during the storage period and restoring the data to a readable state may take one minute. It is suitable for storing archival data, medical images, scientific materials, and video footages for long-term persistence.
6.   In the ACL drop-down box, select an access permission option for the bucket. 
    -   Private: Only the owner of the bucket and the authorized users can perform read, write, and delete operations on the objects in the bucket. Other users cannot access objects in the bucket.
    -   Public Read: Only the owner of the bucket and the authorized users can perform write and delete operations on the objects in the bucket. Anyone \(including anonymous access\) can read the objects in the bucket.
    -   Public Read/Write: Anyone \(including anonymous access\) can read, write, and delete the objects in the bucket.

        **Note:** The fees incurred by such operations are borne by the owner of the bucket. Use this permission with caution.

7.   Click **OK**. 

