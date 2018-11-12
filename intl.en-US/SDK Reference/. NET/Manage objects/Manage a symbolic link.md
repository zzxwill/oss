# Manage a symbolic link {#concept_91929_zh .concept}

A symbolic link is a special object that maps to an object similar to a shortcut used in Windows. You can configure user-defined Object Meta for symbolic links. To obtain a symbolic link, you must have the read permission on it.

Run the following code to create and obtain a symbolic link:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var targetObjectName = "<yourTargetObjectName>";
var symlinkObjectName = "<yourSymlinkObjectName>";
var objectContent = "More than just cloud." ;
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Upload the target file.
    byte[] binaryData = Encoding.ASCII.GetBytes(objectContent);
    MemoryStream requestContent = new MemoryStream(binaryData);
    client.PutObject(bucketName, targetObjectName, requestContent);
    // Create a symbolic link.
    client.CreateSymlink(bucketName, symlinkObjectName, targetObjectName);
    // Obtain a symbolic link.
    var ossSymlink = client.GetSymlink(bucketName, symlinkObjectName);
    Console.WriteLine("Target object is {0}", ossSymlink.Target);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

For more information about creating symbolic links, see [PutSymlink](../../../../reseller.en-US/API Reference/Object operations/PutSymlink.md#).

For more information about obtaining symbolic links, see [GetSymlink](../../../../reseller.en-US/API Reference/Object operations/GetSymlink.md#).

