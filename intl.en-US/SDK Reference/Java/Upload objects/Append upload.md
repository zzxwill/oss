# Append upload {#concept_84784_zh .concept}

This topic describes how to use append upload.

You cannot perform copyObject for appended objects.

Run the following code for append upload:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

String content1 = "Hello OSS A \n";
String content2 = "Hello OSS B \n";
String content3 = "Hello OSS C \n";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

ObjectMetadata meta = new ObjectMetadata();
// Specify the content type of the object you want to upload.
meta.setContentType("text/plain");

// Configure parameters with AppendObjectRequest.
AppendObjectRequest appendObjectRequest = new AppendObjectRequest("<yourBucketName>", "<yourObjectName>", new ByteArrayInputStream(content1.getBytes()),meta);

// Configure a single parameter with AppendObjectRequest.
// Specify the bucket name.
//appendObjectRequest.setBucketName("<yourBucketName>");
// Specify the object name.
//appendObjectRequest.setKey("<yourObjectName>");
// Configure the type of content you want to append. Two types of content are available: InputStream and File. Select InputStream.
//appendObjectRequest.setInputStream(new ByteArrayInputStream(content1.getBytes()));
// Configure the type of content you want to append. Two types of content are available: InputStream and File. Select File.
//appendObjectRequest.setFile(new File("<yourLocalFile>"));
// Specify object meta, because the specified object meta takes effect from the first append.
//appendObjectRequest.setMetadata(meta);

// Start the first append.
// Configure the position where the object is appended based on the previous object length.
appendObjectRequest.setPosition(0L);
AppendObjectResult appendObjectResult = ossClient.appendObject(appendObjectRequest);
// Calculate the 64-bit CRC value. The value is calculated based on the ECMA-182 standard. This value is calculated based on ECMA-182 criteria.
System.out.println(appendObjectResult.getObjectCRC());

// Start the second append.
// nextPosition indicates the position provided for the next request, namely, the length of the current object.
appendObjectRequest.setPosition(appendObjectResult.getNextPosition());
appendObjectRequest.setInputStream(new ByteArrayInputStream(content2.getBytes()));
appendObjectResult = ossClient.appendObject(appendObjectRequest);

// Start the third append.
appendObjectRequest.setPosition(appendObjectResult.getNextPosition());
appendObjectRequest.setInputStream(new ByteArrayInputStream(content3.getBytes()));
appendObjectResult = ossClient.appendObject(appendObjectRequest);

// Close your OSSClient.
ossClient.shutdown();

```

For more information about append upload, see [Append object](../../../../reseller.en-US/Developer Guide/Upload files/Append object.md#) in OSS Developer Guide. For the complete code of append upload, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/AppendObjectSample.java).

