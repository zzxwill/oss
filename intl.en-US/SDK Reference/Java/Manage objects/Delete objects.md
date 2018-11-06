# Delete objects {#concept_84842_zh .concept}

This topic describes how to delete objects.

**Warning:** Delete objects with caution because deleted objects cannot be recovered.

## Delete an object {#section_hzs_y1c_kfb .section}

Run the following code to delete a single object:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Delete an object.
ossClient.deleteObject(bucketName, objectName);

// Close your OSSClient.
ossClient.shutdown();

```

## Delete multiple objects { .section}

You can delete a maximum of 1,000 objects simultaneously. Objects can be deleted in two modes: detail \(verbose\) and simple \(quiet\) modes.

-   verbose: returns the list of objects that you have deleted successfully. The default mode is verbose.
-   quiet: returns the list of objects that you failed to delete.

The following table describes the parameters for DeleteObjectsRequest.

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|Keys|Specifies the objects you want to delete.|setKeys\(List<String\>\)|
|quiet|Specifies a mode that the deletion result uses. True indicates quiet. False indicates verbose. The default mode is verbose.|setQuiet\(boolean\)|
|encodingType|Encodes the name of the returned objects. Only URL is supported.|setEncodingType\(String\)|

The following table describes the parameters for DeleteObjectsResult.

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|deletedObjects|Specifies the deletion result. verbose indicates the list of objects you have deleted successfully. quiet indicates the list of objects you failed to delete.|List<String\> getDeletedObjects\(\)|
|encodingType|Encodes the name of objects in deletedObjects.|getEncodingType\(\)|

Run the following code to delete multiple objects simultaneously:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Delete objects.
List<String> keys = new ArrayList<String>();
keys.add("key0");
keys.add("key1");
keys.add("key2");

DeleteObjectsResult deleteObjectsResult = ossClient.deleteObjects(new DeleteObjectsRequest(bucketName).withKeys(keys));
List<String> deletedObjects = deleteObjectsResult.getDeletedObjects();

// Close your OSSClient.
ossClient.shutdown();

```

For the complete code of deleting multiple objects simultaneously, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/DeleteObjectsSample.java).

