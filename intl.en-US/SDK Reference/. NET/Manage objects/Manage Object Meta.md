# Manage Object Meta {#concept_91920_zh .concept}

Object Meta includes HTTP headers and user-defined Object Meta. For more information, see [Object Meta](../../../../reseller.en-US/Developer Guide/Manage files/Object Meta.md#) in OSS Developer Guide.

For the complete code of configuring Object Meta, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PutObjectSample.cs). For the complete code of modifying Object Meta, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/ModifyObjectMetaSample.cs).

Run the following code to configure, modify, and obtain Object Meta:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
var localFilename = "<yourLocalFilename>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    using (var fs = File.Open(localFilename, FileMode.Open))
    {
        // Create Object Meta. You can configure HTTP headers with Object Meta.
        var metadata = new ObjectMetadata()
        {
            // Specify the object type.
            ContentType = "text/html",
            // Configure the expiration time (GMT is used).
            ExpirationTime = DateTime.Parse("2025-10-12T00:00:00.000Z"),
        };
        // Configure the length of the object you want to upload. If the object length is greater than the configured length, only the configured length of the object is uploaded. If the configured length is greater than the actual object length, the entire object is uploaded.
        metadata.ContentLength = fs.Length;
        // Configure the cache action of a webpage when an object is downloaded.
        metadata.CacheControl = "No-Cache";
        // Set the value of metadata mykey1 to myval1.
        metadata.UserMetadata.Add("mykey1", "myval1");
        // Set the value of metadata mykey2 to myval2.
        metadata.UserMetadata.Add("mykey2", "myval2");
        var saveAsFilename = "Filetest123.txt";
        var contentDisposition = string.Format("attachment;filename*=utf-8''{0}", HttpUtils.EncodeUri(saveAsFilename, "utf-8"));
        // Provide a default file name when saving the requested content as a file.
        metadata.ContentDisposition = contentDisposition;
        // Upload a file and configure the Object Meta.
        client.PutObject(bucketName, objectName, fs, metadata);
        Console.WriteLine("Put object succeeded");
        //Obtain Object Meta.
        var oldMeta = client.GetObjectMetadata(bucketName, objectName);
        // Configure new Object Meta.
        var newMeta = new ObjectMetadata()
        {
            ContentType = "application/octet-stream",
            ExpirationTime = DateTime.Parse("2035-11-11T00:00:00.000Z"),
            // Specify the encoding format of a download object.
            ContentEncoding = null,
            CacheControl = ""
        };
        // Add custom Object Meta.
        newMeta.UserMetadata.Add("author", "oss");
        newMeta.UserMetadata.Add("flag", "my-flag");
        newMeta.UserMetadata.Add("mykey2", "myval2-modified-value");
        // Use the ModifyObjectMeta method to modify Object Meta.
        client.ModifyObjectMeta(bucketName, objectName, newMeta);
    }
}
catch (Exception ex)
{
    Console.WriteLine("Put object failed, {0}", ex.Message);
}
```

**Note:** 

-   For detailed information about HTTP header, see [RFC2616](https://tools.ietf.org/html/rfc2616).
-   The size of Object Meta with custom HTTP headers is no larger than 8 KB.

