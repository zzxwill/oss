# Range download {#concept_91750_zh .concept}

If you only need part of the data in an object, you can use range downloads to download specified content.

For the complete code of range download, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/GetObjectByRangeSample.cs).

Run the following code for range download:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
var downloadFilename = "<yourDownloadFilename>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    var getObjectRequest = new GetObjectRequest(bucketName, objectName);
    // Set the range, which is from the 20th byte to the 100th byte.
    getObjectRequest.SetRange(20, 100);
    // Start range download. The setRange of getObjectRequest can be used to implement multipart download and resumable download.
    var obj = client.GetObject(getObjectRequest);
    // Download the data and write it into a file.
    using (var requestStream = obj.Content)
    {
        byte[] buf = new byte[1024];
        var fs = File.Open(downloadFilename, FileMode.OpenOrCreate);
        var len = 0;
        while ((len = requestStream.Read(buf, 0, 1024)) ! = 0)
        {
            fs.Write(buf, 0, len);
        }
        fs.Close();
    }
    Console.WriteLine("Get object succeeded");
}
catch (Exception ex)
{
    Console.WriteLine("Get object failed. {0}", ex.Message);
}
```

You can configure the following parameters of GetObjectRequest.

|Parameter|Description|
|:--------|:----------|
|Range|Specifies the transmission range of an object.|
|ModifiedSinceConstraint|If the specified time is earlier than the actual modification time of the file, the file is transmitted normally. Otherwise, a 304 Not Modified exception is thrown.|
|UnmodifiedSinceConstraint|If the time passed to the parameter is equal to or later than the actual modification time of the file, the file is transmitted normally Otherwise, a 412 precondition failed exception is thrown.|
|MatchingETagConstraints|Passes in a group of ETags. If the ETags match the ETag of the file, the file is transmitted normally. Otherwise, a 412 precondition failed exception is thrown.|
|NonmatchingEtagConstraints|Passes in a group of ETags. If the ETags do not match the ETag of the file, the file is transmitted normally. Otherwise, a 304 Not Modified exception is thrown.|
|ResponseHeaderOverrides|Customizes some headers in requests returned by OSS.|

