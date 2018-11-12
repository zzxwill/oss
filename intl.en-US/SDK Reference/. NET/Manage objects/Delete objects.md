# Delete objects {#concept_91924_zh .concept}

This topic describes how to delete objects.

**Warning:** Delete objects with caution because deleted objects cannot be recovered.

For the complete code of deleting objects, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/DeleteObjectsSample.cs).

## Delete an object {#section_kbr_nm2_lfb .section}

Run the following code to delete a single object:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    /// Delete an object.
    client.DeleteObject(bucketName, objectName);
    Console.WriteLine("Delete object succeeded");
}
catch (Exception ex)
{
    Console.WriteLine("Delete object failed. {0}", ex.Message);
}
```

## Delete multiple objects { .section}

You can delete a maximum of 1,000 objects simultaneously. Objects can be deleted in two modes: detail \(verbose\) and simple \(quiet\) modes.

-   verbose: returns the list of objects that you have deleted successfully. The default mode is verbose.
-   quiet: returns the list of objects that you failed to delete.

Run the following code to delete multiple objects simultaneously:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    var keys = new List<string>();
    var listResult = client.ListObjects(bucketName);
    foreach (var summary in listResult.ObjectSummaries)
    {
        keys.Add(summary.Key);
    }
    // If the quietMode is true, the quiet mode is used. If the quietMode is false, the verbose mode is used. The default mode is verbose.
    var quietMode = false;
    // The third parameter of the DeleteObjectsRequest specifies the return mode.
    var request = new DeleteObjectsRequest(bucketName, keys, quietMode);
    // Delete multiple objects.
    var result = client.DeleteObjects(request);
    if ((! quietMode) && (result.Keys ! = null))
    {
        foreach (var obj in result.Keys)
        {
            Console.WriteLine("Delete successfully : {0} ", obj.Key);
        }
    }
    Console.WriteLine("Delete objects succeeded");
}
catch (Exception ex)
{
    Console.WriteLine("Delete objects failed. {0}", ex.Message);
}
```

