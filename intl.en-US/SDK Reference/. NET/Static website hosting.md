# Static website hosting {#concept_90311_zh .concept}

You can set your bucket configuration to the static website hosting mode. After the configuration takes effect, you can access this static website with the bucket domain and be redirected to a specified index page or error page.

For more information about static website hosting, see [Static website hosting](../../../../reseller.en-US/Developer Guide/Static website hosting/Configure static website hosting.md#).

## Configure static website hosting {#section_yhd_lfd_kfb .section}

Run the following code to configure static website hosting:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Configure static website hosting.
    var request = new SetBucketWebsiteRequest(bucketName, "index.html", "error.html");
    client.SetBucketWebsite(request);
    Console.WriteLine("Set bucket:{0} Wetbsite succeeded ", bucketName);
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error info: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

## View static website hosting configurations { .section}

Run the following code to view static website hosting configurations:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // View static website hosting configurations.
    var result = client.GetBucketWebsite(bucketName);
    Console.WriteLine("Get bucket:{0} Wetbsite succeeded, index doc:{1}, error doc:{2}",
                      bucketName, result.IndexDocument, result.ErrorDocument);
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error info: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

## Delete static website hosting configurations { .section}

Run the following code to delete static website hosting configurations:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Delete static website hosting configurations.
    client.DeleteBucketWebsite(bucketName);
    Console.WriteLine("Delete bucket:{0} Wetbsite succeeded ", bucketName);
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error info: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}", 
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

