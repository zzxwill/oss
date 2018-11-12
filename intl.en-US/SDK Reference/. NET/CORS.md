# CORS {#concept_32095_zh .concept}

Cross-origin resource sharing \(CORS\) allows web applications to access resources that belong to another region. OSS provides CORS APIs for convenient cross-origin access control.

For more information, see [Cross-origin resource sharing](../../../../reseller.en-US/Developer Guide/Security management/Cross-origin resource sharing.md#) and [PutBucketcors](../../../../reseller.en-US/API Reference/Cross-Origin Resource Sharing/PutBucketcors.md#) in OSS Developer Guide.

For the complete code of CORS, see [GitHub](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/sample/bucket_cors.go).

## Configure CORS rules {#section_i1s_br2_lfb .section}

For complete code of configuring CORS rules, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/SetBucketCorsSample.cs).

Run the following code to configure CORS rules for a specified bucket:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
// Creates an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    var request = new SetBucketCorsRequest(bucketName);
    var rule1 = new CORSRule();
    // Specify the allowed source of the cross-origin access request.
    rule1. AddAllowedOrigin("http://www.a.com");
    // Specify the cross-region request methods (GET, PUT, DELETE, POST, and HEAD) that are allowed.
    rule1. AddAllowedMethod("POST");
    // AllowedHeaders and ExposeHeaders do not support wildcards.
    rule1. AddAllowedHeader("*");
    // Specify the response header that allows user access from applications.
    rule1. AddExposeHeader("x-oss-test");
    // A maximum of 10 rules is allowed.
    request.AddCORSRule(rule1);
    var rule2 = new CORSRule();
    // AllowedOrigins and AllowedMethods allow only one wildcard asterisk (*). Wildcard asterisks (*) indicate that all sources of the cross-origin requests and operations are allowed.
    rule2. AddAllowedOrigin("http://www.b.com");
    rule2. AddAllowedMethod("GET");
    // Determine whether the header specified in the Access-Control-Headers in the pre-flight (OPTIONS) requests is allowed.
    rule2. AddExposeHeader("x-oss-test2");
    // Specify the cache time (seconds) for the response of browser pre-flight (OPTIONS) requests to a specific resource.
    rule2. MaxAgeSeconds = 100;
    request.AddCORSRule(rule2);
    // Configure CORS rules.
    client.SetBucketCors(request);
    Console.WriteLine("Set bucket:{0} Cors succeeded ", bucketName);
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

## Obtain CORS rules { .section}

For the complete code of obtaining CORS rules, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/GetBucketCorsSample.cs).

Run the following code to obtain CORS rules:

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
    // Obtain CORS rules.
    var result = client.GetBucketCors(bucketName);
    Console.WriteLine("Get bucket:{0} Cors succeeded ", bucketName);
    foreach (var rule in result)
    {
        foreach (var origin in rule.AllowedOrigins)
        {
            Console.WriteLine("Allowed origin:{0}", origin);
        }
    }
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

## Delete CORS rules { .section}

For the complete code of deleting CORS rules, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/DeleteBucketCorsSample.cs).

Run the following code to delete all CORS rules for a specified bucket:

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
    // Delete CORS rules.
    client.DeleteBucketCors(bucketName);
    Console.WriteLine("Delete bucket:{0} Cors succeeded ", bucketName);
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error info: {0} ; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

