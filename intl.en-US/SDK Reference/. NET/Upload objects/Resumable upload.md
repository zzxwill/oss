# Resumable upload {#concept_91101_zh .concept}

You can use resumable upload to split a file you want to upload into several parts and upload them simultaneously. After you have uploaded all parts, you can combine these parts into a complete object. Thus, you can complete uploading the entire object.

Current upload progress is recorded into a checkpoint file during the upload. If a part fails to be uploaded during the process, the object is uploaded from the part recorded in the checkpoint file when the upload restarts, that is, the resumable upload. After the upload is complete, the checkpoint is deleted.

For the complete code of resumable upload, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/ResumableSample.cs).

Run the following code for resumable upload:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
var localFilename = "<yourLocalFilename>";
string checkpointDir = "<yourCheckpointDir>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    //  Configure parameters with UploadFileRequest.
    UploadObjectRequest request = new UploadObjectRequest(bucketName, objectName, localFilename)
    {
        // Specify the size of a part you want to upload.
        PartSize = 8 * 1024 * 1024,
        // Specify the number of threads for simultaneous upload.
        ParallelThreadCount = 3,
        // The progress information of a resumable upload is stored in the checkpointDir directory.  If a part fails to be uploaded, the progress information is used to continue the upload. If the checkpointDir directory is null, the resumable upload does not take effect, and the file that failed to be uploaded is uploaded all over again.
        CheckpointDir = checkpointDir,
    };
    // Start resumable upload.
    client.ResumableUploadObject(request);
    Console.WriteLine("Resumable upload object:{0} succeeded", objectName);
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

