# Streaming download {#concept_84823_zh .concept}

If you have a large object to download or it is time-consuming to download the entire object at a time, you can use streaming download. Streaming download enables you to download part of the object each time until you have downloaded the entire object.

If you do not close OssObject after it is used, connection leaks may occur. Consequently, no connection is available and the program cannot run properly. Run the following code to close OssObject:

```language-java
OSSObject ossObject = ossClient.getObject(bucketName, objectName);
ossObject.close();

```

The following code is used for streaming downloads:

```language-java
// This example uses the endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// ossObject includes the bucket name, key (object name), object meta (meta information), and InputStream.
OSSObject ossObject = ossClient.getObject(bucketName, objectName);

// Read the object content.
System.out.println("Object content:");
BufferedReader reader = new BufferedReader(new InputStreamReader(ossObject.getObjectContent()));
while (true) {
    String line = reader.readLine();
    if (line == null) break;

    System.out.println("\n" + line);
}
// If you do not close the reader after the data is read, connection leaks may occur. Consequently, no connection is available and the program cannot run properly.
reader.close();

//Close your OSSClient.
ossClient.shutdown();

```

For the complete code of streaming download, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/SimpleGetObjectSample.java).

