# Create and use the Archive bucket {#concept_wx5_szt_tdb .concept}

OSS provides three storage classes, among which the Archive storage class has the lowest price. However, you have to restore the archived data to a readable state if you want to access it. This topic describes how to create and use the bucket of the Archive storage class. For more information about the three storage classes, see [Introduction to storage classes](intl.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#).

## Create an Archive bucket {#section_cw1_vyv_tdb .section}

You can choose to create an Archive bucket by using the console, APIs/SDKs, or command line tools.

-   Create an Archive bucket by using the console

    To create an Archive bucket in the console, select Archive for Storage Class.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4342/1015_en-US.png)

-   Create an Archive bucket by using APIs/SDKs

    Take the Java SDK for example:

    ```
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    CreateBucketRequest createBucketRequest=new CreateBucketRequest(bucketName);
    // Set the bucket ACL to public-read. The default ACL policy is private-read-write. createBucketRequest.setCannedACL(CannedAccessControlList.PublicRead);
    // Set the storage class to Archive. The default storage class is Standard.
    createBucketRequest.setStorageClass(StorageClass.Archive);
    ossClient.createBucket(createBucketRequest);
    ```

    `createBucketRequest.setStorageClass(StorageClass.Archive);` indicates that the storage class of the created bucket is Archive.

-   Create an Archive bucket by using OSS command line tools

    Take ossutil for example:

    ```
    ./ossutil mb oss://[bucket name] --storage-class=Archive
    ```

    Replace `[bucket name]` with your own bucket. Set `--storage-class` to `Archive`.


## Use an Archive bucket {#section_omm_n1w_tdb .section}

-   Upload data to an Archive bucket

    Archive buckets support PutObject and MultipartUpload, but do not support AppendObject. The objects uploaded by PutObject and MultipartUpload can be directly stored in Archive buckets.

-   Download data from an Archive bucket

    Objects stored in Archive buckets are not accessible in real time. You must first initiate a restore request and then wait for one minute until the object is available.

    The restore process of an archived object is as follows: 

    1.  The archived object is in the frozen state at the beginning.
    2.  After a restore request is submitted, the object enters the restoring state.
    3.  One minute later, the object enters the restored state and can be read.
    4.  The restored state lasts for one day by default, and can be prolonged to a maximum of seven days. Once this period ends, the object returns to the frozen state.

You can choose to restore an archived object by using the console, APIs/SDKs, or command line tools.

-   Restore an archived object by using the console

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4342/1024_en-US.png)

    To restore an archived object in the console, enter the Preview page of the object, and then click **Restore**. It takes one minute to restore the object. During this process, the object is in the restoring state.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4342/1025_en-US.png)

-   Restore an archived object by using APIs/SDKs

    Take the Java SDK for example. Call the restoreObject method to restore an object:

    ```
    ObjectMetadata objectMetadata = ossClient.getObjectMetadata(bucketName, key);
    // check whether the object is archive class
    StorageClass storageClass = objectMetadata.getObjectStorageClass();
    if (storageClass == StorageClass.Archive) {
        // restore object
        ossClient.restoreObject(bucketName, key);
        // wait for restore completed
        do {
            Thread.sleep(1000);
            objectMetadata = ossClient.getObjectMetadata(bucketName, key);
        } while (! objectMetadata.isRestoreCompleted());
    
    // get restored object
    OSSObject ossObject = ossClient.getObject(bucketName, key);
    ossObject.getObjectContent().close();
    ```

-   Restore an archived object by using OSS command line tools

    Take ossutil for example:

    ```
    ./ossutil restore oss://[Bucket name]/[Object name]
    ```

    Replace `[Bucket name]` and `[Object name]` with your own bucket and object.


