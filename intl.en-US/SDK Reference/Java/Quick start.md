# Quick start {#concept_32011_zh .concept}

This topic describes how to use OSS Java SDK to perform routine operations such as bucket creation, object uploads, and object downloads.

## Sample project {#section_kpb_n4y_rfb .section}

OSS Java SDK provides Maven-based and Ant-based sample projects. You can compile and run the sample projects on local devices or develop your own applications based on the sample projects. For the compilation and running method of the projects, see the README.md in the project directory.

-   Maven sample project: [aliyun-oss-java-sdk-demo-mvn.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/92588/APP_zh/1538983135359/aliyun-oss-java-sdk-demo-mvn.zip).
-   Ant sample project:[aliyun-oss-java-sdk-demo-ant.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/92588/APP_zh/1538983246527/aliyun-oss-java-sdk-demo-ant.zip).

## Create a bucket { .section}

A bucket is a global namespace in OSS. It is similar to a data container that stores files. Run the following code to create a bucket:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Create a bucket.
ossClient.createBucket(bucketName);

// Close your OSSClient instance.
ossClient.shutdown();

```

For more information about bucket naming rules, see naming conventions in [Basic concepts](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#). For more information about how to create a bucket, see [Manage a bucket](reseller.en-US/SDK Reference/Java/Manage a bucket.md#).

For more information about endpoints, see [Regions and endpoints](../../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).

## Upload objects { .section}

Run the following code to upload a file to OSS:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Upload content to the specified bucket and save it as the specified object name.
String content = "Hello OSS";
ossClient.putObject(bucketName, objectName, new ByteArrayInputStream(content.getBytes()));

// Close your OSSClient instance.
ossClient.shutdown();

```

For more information, see [Upload objects](reseller.en-US/SDK Reference/Java/Upload objects/Overview.md#).

## Download objects { .section}

Run the following code to obtain object content:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Call ossClient.getObject to return an OSSObject instance. The OSSObject instance contains the object content and Object Meta.
OSSObject ossObject = ossClient.getObject(bucketName, objectName);
// Call ossObject.getObjectContent to obtain object InputStream. Read the InputStream to obtain object content.
InputStream content = ossObject.getObjectContent();
if (content ! = null) {
    BufferedReader reader = new BufferedReader(new InputStreamReader(content));
    while (true) {
        String line = reader.readLine();
        if (line == null) break;
        System.out.println("\n" + line);
    }
    // If you do not close the reader after the data is read, connection leaks may occur. Consequently, no available connections are left and an exception occurs.
    content.close();
}

// Close your OSSClient instance.
ossClient.shutdown();

```

For more information, see [Download objects](reseller.en-US/SDK Reference/Java/Download objects/Overview.md#).

## List objects { .section}

Use the following code to list objects in a bucket. You can list a maximum of 100 objects by default.

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Use ossClient.listObjects to return an ObjectListing instance. The ObjectListing instance contains the response from listObject.
ObjectListing objectListing = ossClient.listObjects(bucketName);
// Use objectListing.getObjectSummaries to obtain the descriptions of all objects.
for (OSSObjectSummary objectSummary : objectListing.getObjectSummaries()) {
    System.out.println(" - " + objectSummary.getKey() + "  " +
            "(size = " + objectSummary.getSize() + ")");
}

// Close your OSSClient instance.
ossClient.shutdown();

```

For more information, see [List objects](reseller.en-US/SDK Reference/Java/Manage objects/List objects.md#) in [Manage objects](reseller.en-US/SDK Reference/Java/Manage objects/Overview.md#).

## Delete objects { .section}

For the complete code of deleting objects, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/GetStartedSample.java).

Use the following code to delete a specified object:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Delete objects.
ossClient.deleteObject(bucketName, objectName);

// Close your OSSClient instance.
ossClient.shutdown();

```

For more information, see [Delete objects](reseller.en-US/SDK Reference/Java/Manage objects/Delete objects.md#) in [Manage objects](reseller.en-US/SDK Reference/Java/Manage objects/Overview.md#).

