# Manage lifecycle rules {#concept_32094_zh .concept}

You can use OSS to configure lifecycle rules to reduce storage costs. The lifecycle management rules can be used for automatic deletion of expired objects and fragments, or storage class conversion to Infrequent Access for objects that will expire soon. Each rule includes:

-   Rule ID: It identifies a rule. Ensure that each rule ID in a bucket is unique.
-   Policy: You can configure a policy by using the following configuration methods. Only one method can be configured for a bucket.
    -   Configure by Prefix: You can create multiple rules with this method. Ensure that each prefix is unique.
    -   Configure for Entire Bucket: You can configure only one rule with this method.
-   Expired: You can configure the following configuration methods:
    -   Set by Number of Days: It specifies the number of days from the last modification time of objects.
    -   Set by Date: It specifies the date prior to which objects are created. If objects are created prior to the date, these objects are deleted.
-   Status: This lifecyle rule takes effect or not.

The parts uploaded by uploadPart method also support lifecycle rules. The last modification time of an object is the time the multipart upload event is initiated.

For more information about lifecycle management, see [Manage object lifecycle](../../../../reseller.en-US/Developer Guide/Manage files/Manage object lifecycle.md#).

For the complete code of lifecycle management, see [GitHub](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/sample/bucket_lifecycle.go).

## Configure lifecycle rules {#section_mm2_wcd_kfb .section}

Run the following code to configure lifecycle rules:

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
    var setBucketLifecycleRequest = new SetBucketLifecycleRequest(bucketName);
    // Create the first lifecycle rule that is valid within 3 days.
    LifecycleRule lcr1 = new LifecycleRule()
    {
        ID = "delete obsoleted files",
        Prefix = "obsoleted/",
        Status = RuleStatus.Enabled,
        ExpriationDays = 3
    };
    // Create the second lifecycle rule.
    LifecycleRule lcr2 = new LifecycleRule()
    {
        ID = "delete temporary files",
        Prefix = "temporary/",
        Status = RuleStatus.Enabled,
        ExpriationDays = 20
    };
    setBucketLifecycleRequest.AddLifecycleRule(lcr1);
    setBucketLifecycleRequest.AddLifecycleRule(lcr2);
    // Configure the lifecycle.
    client.SetBucketLifecycle(setBucketLifecycleRequest);
    Console.WriteLine("Set bucket:{0} Lifecycle succeeded ", bucketName);
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

## View lifecycle rules { .section}

For the complete code of viewing lifecycle rules, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/GetBucketLifecycleSample.cs).

Run the following code to view lifecycle rules:

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
    // View the lifecycle rules.
    var rules = client.GetBucketLifecycle(bucketName);
    Console.WriteLine("Get bucket:{0} Lifecycle succeeded ", bucketName);
    foreach (var rule in rules)
    {
        Console.WriteLine("ID: {0}", rule.ID);
        Console.WriteLine("Prefix: {0}", rule.Prefix);
        Console.WriteLine("Status: {0}", rule.Status);
        if (rule.ExpriationDays.HasValue)
            Console.WriteLine("ExpirationDays: {0}", rule.ExpriationDays);
    }
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

## Clear lifecycle rules { .section}

Run the following code to clear lifecycle rules:

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
    // Clear the lifecycle rules
    client.DeleteBucketLifecycle(bucketName);
    Console.WriteLine("Delete bucket:{0} Lifecycle succeeded ", bucketName);
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

