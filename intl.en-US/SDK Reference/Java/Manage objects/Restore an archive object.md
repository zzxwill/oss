# Restore an archive object {#concept_84846_zh .concept}

This topic describes how to restore an archive object.

For the complete code of restoring an archive object, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/test/java/com/aliyun/oss/integrationtests/ArchiveTest.java).

You must restore an archive object before you read it. Do not call restoreObject for non-archive objects.

The state conversion process of an archive object is as follows:

1.  An archive object is in the frozen state.
2.  After you submit it for restoration, the server restores the object. The object is in the restoring state.
3.  You can read the object after it is restored. The restored state of the object lasts one day by default. You can prolong this period to a maximum of seven days. Once this period expires, the object returns to the frozen state.

Run the following code to restore an object:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

ObjectMetadata objectMetadata = ossClient.getObjectMetadata(bucketName, key);

// Verify whether the object is an archive object.// Verify that the file is an archive file.
StorageClass storageClass = objectMetadata.getObjectStorageClass();
if (storageClass == StorageClass.Archive) {
    // Restore the object.
    ossClient.restoreObject(bucketName, objectName);
    
    // Wait until the object is restored.
    do {
        Thread.sleep(1000);
        objectMetadata = ossClient.getObjectMetadata(bucketName, objectName);
    } while (! objectMetadata.isRestoreCompleted());
}

// Obtain the restored object.
OSSObject ossObject = ossClient.getObject(bucketName, objectName);
ossObject.getObjectContent().close();

// Close your OSSClient.
ossClient.shutdown();

```

For more information about the archive storage classes, see [Introduction to storage classes](../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#).

