# Resumable download {#concept_84827_zh .concept}

Large-sized objects may fail to be downloaded because the network is unstable or the Java program exits abnormally and the entire object needs to be downloaded again. However, the object may still fail to be downloaded after multiple attempts. Therefore, OSS provides resumable download.

To enable resumable download, you need to split the object you want to download into multiple parts and download them separately. After you have downloaded all parts, these parts will be combined into a complete object.

You can use ossClient.downloadFile to enable resumable download. DownloadFileRequest contains the following parameters:

|Parameter|Description|Required or optional|Default value|Configuration methods|
|:--------|:----------|:-------------------|:------------|:--------------------|
|bucket|Specifies the name of a bucket.|Required|None|Constructor|
|key|Specifies the name of an object.|Required|None|Constructor|
|downloadFile|Specifies a local file. You can download an object to the local file.|Optional|Name of the object|Constructor or setDownloadFile|
|partSize|Specifies the size of a part. The value can be between 1 byte to 5 GB.|Optional|Size of an object/10000|setPartSize|
|taskNum|Specifies the number of parts you need to download simultaneously.|Optional|1|setTaskNum|
|enableCheckpoint|Specifies whether resumable download is enabled.|Optional|Disable|setEnableCheckpoint|
|checkpointFile|Specifies the file to record the results of partial downloads. You need to configure this parameter to enable resumable download. This file stores information about the download progress. If you fail to download a part, the download will continue based on the recorded progress. After the object is downloaded to your local device, this file will be deleted.|Optional|downloadFile.ucp \(share the same directory with DownloadFile\)|setCheckpointFile|

Run the following code for resumable download:

```language-java
//This example uses the endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
//It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Send a request to download 10 tasks simultaneously and start resumable download.
DownloadFileRequest downloadFileRequest = new DownloadFileRequest(bucketName, objectName);
downloadFileRequest.setDownloadFile("<yourDownloadFile>");
downloadFileRequest.setPartSize(1 * 1024 * 1024);
downloadFileRequest.setTaskNum(10);
downloadFileRequest.setEnableCheckpoint(true);
downloadFileRequest.setCheckpointFile("<yourCheckpointFile>");

// Download the object to your local file.
DownloadFileResult downloadRes = ossClient.downloadFile(downloadFileRequest);
// After the download succeeds, object meta is returned.
downloadRes.getObjectMeta();

//Close your OSSClient.
ossClient.shutdown();

```

