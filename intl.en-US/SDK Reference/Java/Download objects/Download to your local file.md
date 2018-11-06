# Download to your local file {#concept_84824_zh .concept}

This topic describes how to download an object from OSS to a local file.

You can run the following code to download a specified object from OSSClient to your local file:

```language-java
//This example uses the endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Download an object to your local file. If the name of the object is the same as that of your local file, the object will replace the local file. If the name of the object is different from that of your local file, the object will be downloaded and a new file will be added to your local device.
ossClient.getObject(new GetObjectRequest(bucketName, objectName), new File("<yourLocalFile>"));

//Close your OSSClient.
ossClient.shutdown();

```

