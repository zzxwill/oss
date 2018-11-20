# Range download {#concept_84825_zh .concept}

If you only need part of the data in an object, you can use range downloads to download specified content.

Run the following code for range download:

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

GetObjectRequest getObjectRequest = new GetObjectRequest(bucketName, objectName);
// Obtain data between the starting byte value of 0 and the maximum byte value of 1,000 inclusive (that is, a total of 1,001 bytes). If the specified value range is invalid, for example, the specified range includes a negative number, or the specified value is larger than the object size, the entire object is downloaded.
getObjectRequest.setRange(0, 1000);

// Start range download.
OSSObject ossObject = ossClient.getObject(getObjectRequest);

// Read data.
byte[] buf = new byte[1024];
InputStream in = ossObject.getObjectContent();
for (int n = 0; n ! = -1; ) {
    n = in.read(buf, 0, buf.length);
}

// If you do not close the reader after the data is read, connection leaks may occur. Consequently, no connections are available and an exception occurs.
in.close();

//Close your OSSClient.
ossClient.shutdown();

```

All data cannot be read at one time when it is streamed. To stream a 64 KB block of data, use the following code to read the data multiple times until the entire object is downloaded. For more information, see [InputStream.read](https://docs.oracle.com/javase/7/docs/api/java/io/InputStream.html).

```
byte[] buf = new byte[1024];
InputStream in = ossObject.getObjectContent();
for (int n = 0; n ! = -1; ) {
    n = in.read(buf, 0, buf.length);
}
in.close();

```

