# Create and use Archive buckets {#concept_wx5_szt_tdb .concept}

OSS provides three [storage classes](reseller.en-US/Developer Guide/Storage classes/Overview.md#). This topic describes how to create and use Archive buckets.

## Create an Archive bucket {#section_cw1_vyv_tdb .section}

You can use the console, APIs or SDKs, or command-line tools to create an Archive bucket.

-   Console

    On the Create Bucket page of the OSS console, set Storage Class to Archive, as shown in the following figure.

-   APIs or SDKs

    The following code uses the Java SDK as an example:

    ```
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    CreateBucketRequest createBucketRequest=new CreateBucketRequest(bucketName);
    // Set the bucket ACL to public-read. The default ACL is private. createBucketRequest.setCannedACL(CannedAccessControlList.PublicRead);
    // Set the storage class to Archive. The default storage class is Standard.
    createBucketRequest.setStorageClass(StorageClass.Archive);
    ossClient.createBucket(createBucketRequest);
    ```

    `createBucketRequest.setStorageClass(StorageClass.Archive);` indicates that the storage class of the created bucket is Archive.

-   Command-line tools

    The following code uses ossutil as an example:

    ```
    ./ossutil mb oss://[Bucket name] --storage-class=Archive
    ```

    Replace `[Bucket name]` with the name of your bucket to be created. Set `--storage-class` to `Archive` to create an Archive bucket.


## Use an Archive bucket {#section_omm_n1w_tdb .section}

-   Upload data

    You can use the PutObject and MultipartUpload APIs to upload data to an Archive bucket. The AppendObject API is not supported. Objects uploaded by using the PutObject and MultipartUpload APIs can be directly stored in Archive buckets.

-   Download data

    Different from objects stored in Standard and infrequent access \(IA\) buckets, objects stored in Archive buckets are not accessible in real time. To read an Archive object, you must first submit a restore request and then wait for 1 minute until the data is readable.

    The restore process of an Archive object is as follows:

    1.  The object is initially in the frozen state.
    2.  After you submit a restore request, the server starts to restore data, and the object enters the restoring state.
    3.  One minute later, the object is restored and enters the readable state.
    4.  The readable status lasts for one day by default, and can be prolonged to a maximum of seven days. After this period, the object returns to the frozen status.

You can restore an Archive object in any of the following ways:

-   Console

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4342/15573860451024_en-US.png)

    Click Preview in the Actions column corresponding to the destination object. On the Preview page, click Restore. The restore operation takes about 1 minute. During this process, the object is in the restoring state.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4342/15573860451025_en-US.png)

-   APIs or SDKs

    The following code uses the Java SDK as an example to show how to call the restoreObject method to restore an object:

    ```
    ObjectMetadata objectMetadata = ossClient.getObjectMetadata(bucketName, key);
    // Check whether the storage class of the object is Archive.
    StorageClass storageClass = objectMetadata.getObjectStorageClass();
    if (storageClass == StorageClass.Archive) {
        // Restore the object.
        ossClient.restoreObject(bucketName, key);
        // Wait until the restore operation is completed.
        do {
            Thread.sleep(1000);
            objectMetadata = ossClient.getObjectMetadata(bucketName, key);
        } while (! objectMetadata.isRestoreCompleted());
    }
    // Obtain the restored object.
    OSSObject ossObject = ossClient.getObject(bucketName, key);
    ossObject.getObjectContent().close();
    ```

-   Command-line tools

    The following code uses ossutil as an example:

    ```
    ./ossutil restore oss://[Bucket name]/[Object name]
    ```

    `Replace [Bucket name]` and `[Object name]` with the names of your bucket and object to be restored.


