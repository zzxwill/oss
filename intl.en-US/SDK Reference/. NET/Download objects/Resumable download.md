# Resumable download {#concept_91756_zh .concept}

Large-sized objects may fail to be downloaded because the network is unstable or the Java program exits abnormally and the entire object needs to be downloaded again. However, the object may still fail to be downloaded after multiple attempts. Therefore, OSS provides resumable download.

Run the following code for resumable download:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;

var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
var localFilename = "<yourLocalFilename>";
var downloadFilename = "<yourDownloadFilename>";
var checkpointDir = "<yourCheckpointDir>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Configure parameters with DownloadObjectRequest.
    DownloadObjectRequest request = new DownloadObjectRequest(bucketName, objectName, downloadFilename)
    {
        // Specify the size of a part you want to download.
        PartSize = 8 * 1024 * 1024,
        // Specifies the number of concurrent download threads.
        ParallelThreadCount = 3,
        // Specify the checkpointDir to store the download progress information. If the download fails, it can be continued based on the progress information. If the checkpointDir directory is null, the resumable upload is not enabled, which means that objects that fails to be downloaded are downloaded all over again.
        CheckpointDir = checkpointDir,
    };
    // Start resumable download.
    client.ResumableDownloadObject(request);
    Console.WriteLine("Resumable download object:{0} succeeded", objectName);
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

For more information about resumable download, see [Resumable download](../../../../reseller.en-US/Developer Guide/Download files/Multipart download.md#) in OSS Developer Guide.

