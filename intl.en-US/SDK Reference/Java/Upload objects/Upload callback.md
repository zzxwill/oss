# Upload callback {#concept_84798_zh .concept}

This topic describes how to use

For the complete code of upload callback, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/CallbackSample.java).

Run the following code for upload callback:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";
// Specify the IP address of the server you want to send the callback request to, for example, http://oss-demo.aliyuncs.com:23450 or http://127.0.0.1:9090.
String callbackUrl = "<yourCallbackServerUrl>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

String content = "Hello OSS";
PutObjectRequest putObjectRequest = new PutObjectRequest(bucketName, objectName,new ByteArrayInputStream(content.getBytes()));

// Configure upload callback parameters.
Callback callback = new Callback();
callback.setCallbackUrl(callbackUrl);
// Configure the value of the host field carried in the callback request header, such as oss-cn-hangzhou.aliyuncs.com.
callback.setCallbackHost("oss-cn-hangzhou.aliyuncs.com");
// Configure the value of the body field carried in the callback request.
callback.setCallbackBody("{\\\"mimeType\\\":${mimeType},\\\"size\\\":${size}}");
// Configure Content-Type for the callback request.
callback.setCalbackBodyType(CallbackBodyType.JSON);
// Configure the custom parameters used to initiate a callback request. Each custom parameter consists of a key and a value. The key must start with x:.
callback.addCallbackVar("x:var1", "value1");
callback.addCallbackVar("x:var2", "value2");
putObjectRequest.setCallback(callback);

PutObjectResult putObjectResult = ossClient.putObject(putObjectRequest);

// Read the message returned from upload callback.
byte[] buffer = new byte[1024];
putObjectResult.getCallbackResponseBody().read(buffer);
// If you do not close the reader after the data is read, connection leaks may occur. Consequently, no available connections are left and an exception occurs.
putObjectResult.getCallbackResponseBody().close();

// Close your OSSClient instance.
ossClient.shutdown();

```

For more information, see [Upload callback](../../../../reseller.en-US/Developer Guide/Upload files/Upload callback.md#) in OSS Developer Guide.

