# Resumable upload {#concept_84785_zh .concept}

You can use resumable upload to split a file you want to upload into several parts and upload them simultaneously. After you have uploaded all parts, you can combine these parts into a complete object. Thus, you can complete uploading the entire object.

For more information about resumable upload, see [Resumable upload](../../../../reseller.en-US/Developer Guide/Upload files/Multipart upload.md#) in the OSS Developer Guide. For the complete code of resumable upload, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/UploadSample.java).

Run the following code for resumable upload:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

ObjectMetadata meta = new ObjectMetadata();
// Specify the content type of the object you want to upload.
meta.setContentType("text/plain");

// Configure parameters with UploadFileRequest.
UploadFileRequest uploadFileRequest = new UploadFileRequest("<yourBucketName>","<yourObjectName>");

// Configure parameters with UploadFileRequest.
// Specify the bucket name.
//uploadFileRequest.setBucketName("<yourBucketName>");
// Specify the object name.
//uploadFileRequest.setKey("<yourObjectName>");
// Specify the local file you want to upload.
uploadFileRequest.setUploadFile("<yourLocalFile>");
// Specify the number of threads for simultaneous upload. The default value is 1.
uploadFileRequest.setTaskNum(5);
// Specify the size of a part you want to upload. The value can be from 100 KB to 5 GB. The default size of a part is calculated as: file size/10,000.
uploadFileRequest.setPartSize(1 * 1024 * 1024);
// Start resumable upload (that is disabled by default.).
uploadFileRequest.setEnableCheckpoint(true);
// Configure the file used to record the result of local part uploads. You need to configure this parameter when you enable resumable upload. This file stores progress information. If a part upload fails, it can be continued based on the progress information recorded in this file. After you have uploaded the file, the file is deleted. By default, this file shares the same directory (uploadFile.ucp) as the file you want to upload.
uploadFileRequest.setCheckpointFile("<yourCheckpointFile>");
// Configure object meta.
uploadFileRequest.setObjectMetadata(meta);
// Configure callback. Set the yourCallbackEvent parameter to Callback.
uploadFileRequest.setCallback("<yourCallbackEvent>");

// Start resumable upload.
ossClient.uploadFile(uploadFileRequest);

// Close your OSSClient.
ossClient.shutdown();

```

