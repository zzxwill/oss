# Authorized access {#concept_32093_zh .concept}

## Use STS for temporary access authorization {#section_nyk_bzc_kfb .section}

OSS supports Alibaba Cloud Security Token Service \(STS\) for temporary access authorization. STS is a web service that provides a temporary access token to a cloud computing user. Through the STS, you can assign a third-party application or a RAM user \(you can manage the user ID\) an access credential with a custom validity period and permissions. For more information about STS, see [STS introduction](../../../../reseller.en-US/API reference/API reference (STS)/Introduction.md#).

STS advantages:

-   Your long-term key \(AccessKey\) is not exposed to a third-party application. You only need to generate an access token and send the access token to the third-party application. You can customize access permissions and the validity of this token.
-   You do not need to keep track of permission revocation issues. The access token automatically becomes invalid when it expires.

For more information about the process of access to OSS with STS, see [RAM and STS scenario practices](../../../../reseller.en-US/Developer Guide/Access and control/Access control.md#) in OSS Developer Guide.

Run the following code to create a signature request with STS:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var securityToken = "<yourSecurityToken>";
// After a user obtains a temporary STS credential, the OSSClient is generated with the security token and temporary access key (AccessKeyID and AccessKeySecret).
// Create an OSSClient instance.
var ossStsClient = new OssClient(endpoint, accessKeyId, accessKeySecret, securityToken);
// Perform operations on OSS.
```

## Sign a URL to authorize temporary access {#section_dpf_dp2_lfb .section}

You can provide a signed URL to a visitor for temporary access. When you sign a URL, you can specify the expiration time for a URL to restrict the period of access from visitors.

For the complete code of signing a URL to authorize temporary access, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/UrlSignatureSample.cs).

-   Use a signed URL to upload a file

    Run the following code to upload a file with a signed URL:

    ```
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
    var endpoint = "<yourEndpoint>";
    var accessKeyId = "<yourAccessKeyId>";
    var accessKeySecret = "<yourAccessKeySecret>";
    var bucketName = "<yourBucketName>";
    var objectName = "<yourObjectName>";
    var objectContent = "More than just cloud." ;
    // Create an OSSClient instance.
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
    try
    {
        // Generate a signed URL for upload.
        var generatePresignedUriRequest = new GeneratePresignedUriRequest(bucketName, objectName, SignHttpMethod.Put)
        {
            Expiration = DateTime.Now.AddHours(1),
        };
        var signedUrl = client.GeneratePresignedUri(generatePresignedUriRequest);
        // Upload a file with the signed URL.
        var buffer = Encoding.UTF8. GetBytes(objectContent);
        using (var ms = new MemoryStream(buffer))
        {
            client.PutObject(signedUrl, ms);
        }
        Console.WriteLine("Put object by signatrue succeeded. {0} ", signedUrl.ToString());
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

-   Use a signed URL to download an object

    Run the following code to download an object with a signed URL:

    ```
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
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
        var metadata = client.GetObjectMetadata(bucketName, objectName);
        var etag = metadata.ETag;
        // Generate a signed URL for download.
        var req = new GeneratePresignedUriRequest(bucketName, objectName, SignHttpMethod.Get);
        var uri = client.GeneratePresignedUri(req);
        // Download an object with the signed URL.
        OssObject ossObject = client.GetObject(uri);
        using (var file = File.Open(downloadFilename, FileMode.OpenOrCreate))
        {
            using (Stream stream = ossObject.Content)
            {
                int length = 4 * 1024;
                var buf = new byte[length];
                do
                {
                    length = stream.Read(buf, 0, length);
                    file.Write(buf, 0, length);
                } while (length ! = 0);
            }
        }
        Console.WriteLine("Get object by signatrue succeeded. {0} ", uri.ToString());
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


