# Multipart upload {#concept_91103_zh .concept}

For the complete code of multipart upload, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/MultipartUploadSample.cs).

To enable multipart upload, perform the following steps:

1.  Initiate a multipart upload event.

    You can call OssClient.initiateMultipartUpload to return the globally unique uploadId created in OSS.

2.  Upload parts.

    You can call OssClient.uploadPart to upload part data.

    **Note:** 

    -   For parts with a same uploadId, parts are sequenced by their part numbers. If you have uploaded a part and use the same part number to upload another part, the later part will replace the former part.
    -   OSS places the MD5 value of part data in ETag and returns the MD5 value to the user.
    -   SDK automatically configures Content-MD5. OSS calculates the MD5 value of uploaded data and compares it with the MD5 value calculated by SDK. If the two values vary, the error code of InvalidDigest is returned.
3.  Complete multipart upload.

    After you have uploaded all parts, call partossClient.completeMultipartUpload to combine these parts into a complete object.


The following code is used as a complete example that describes the process of multipart upload:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
var localFilename = "<yourLocalFilename>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
// Initiate the multipart upload.
var uploadId = "";
try
{
    // Specifiy the name and the bucket of the uploaded object. You can configure ObjectMeta when using InitiateMultipartUploadRequest. However, you do not need to specify the ContentLength.
    var request = new InitiateMultipartUploadRequest(bucketName, objectName);
    var result = client.InitiateMultipartUpload(request);
    uploadId = result.UploadId;
    // Print the UploadId
    Console.WriteLine("Init multi part upload succeeded");
    Console.WriteLine("Upload Id:{0}", result.UploadId);
}
catch (Exception ex)
{
    Console.WriteLine("Init multi part upload failed, {0}", ex.Message);
}
// Calculate the total number of parts.
var partSize = 100 * 1024;
var fi = new FileInfo(localFilename);
var fileSize = fi.Length;
var partCount = fileSize / partSize;
if (fileSize % partSize ! = 0)
{
    partCount++;
}
// Start the multipart upload. partETags is a list of partETags. OSS verifies the validity of all parts one by one after it receives the partETags. After part verification is successful, OSS combines these parts into a complete object.
var partETags = new List<PartETag>();
try
{
    using (var fs = File.Open(localFilename, FileMode.Open))
    {
        for (var i = 0; i < partCount; i++)
        {
            var skipBytes = (long)partSize * i;
            // Locate to the start position of the upload.
            fs.Seek(skipBytes, 0);
            // Calculate the size of the uploaded part. The size of the last part is the size of the data remained after being splited by the part size.
            var size = (partSize < fileSize - skipBytes) ? partSize : (fileSize - skipBytes);
            var request = new UploadPartRequest(bucketName, objectName, uploadId)
            {
                InputStream = fs,
                PartSize = size,
                PartNumber = i + 1
            };
            // Call the UploadPart interface to upload the object. The returned results contain the ETag value of the uploaded part.
            var result = client.UploadPart(request);
            partETags.Add(result.PartETag);
            Console.WriteLine("finish {0}/{1}", partETags.Count, partCount);
        }
        Console.WriteLine("Put multi part upload succeeded");
    }
}
catch (Exception ex)
{
    Console.WriteLine("Put multi part upload failed, {0}", ex.Message);
}
// List uploaded parts.
try
{
    var listPartsRequest = new ListPartsRequest(bucketName, objectName, uploadId);
    var listPartsResult = client.ListParts(listPartsRequest);
    Console.WriteLine("List parts succeeded");
    // Upload each part simultaneously until all parts are uploaded.
    var parts = listPartsResult.Parts;
    foreach (var part in parts)
    {
        Console.WriteLine("partNumber: {0}, ETag: {1}, Size: {2}", part.PartNumber, part.ETag, part.Size);
    }
}
catch (Exception ex)
{
    Console.WriteLine("List parts failed, {0}", ex.Message);
}
// Complete the multipart upload.
try
{
    var completeMultipartUploadRequest = new CompleteMultipartUploadRequest(bucketName, objectName, uploadId);
    foreach (var partETag in partETags)
    {
        completeMultipartUploadRequest.PartETags.Add(partETag);
    }
    var result = client.CompleteMultipartUpload(completeMultipartUploadRequest);
    Console.WriteLine("complete multi part succeeded");
}
catch (Exception ex)
{
    Console.WriteLine("complete multi part failed, {0}", ex.Message);
}
```

## Cancel a multipart upload event {#section_vfs_522_lfb .section}

For the complete code of canceling a multiplart upload event, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/MultipartUploadSample.cs).

Run the following code to cancel a multipart upload event:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
var uploadId = "";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
// Initiate the multipart upload.
try
{
    var request = new InitiateMultipartUploadRequest(bucketName, objectName);
    var result = client.InitiateMultipartUpload(request);
    uploadId = result.UploadId;
    // Print the UploadId.
    Console.WriteLine("Init multi part upload succeeded");
    Console.WriteLine("Upload Id:{0}", result.UploadId);
}
catch (Exception ex)
{
    Console.WriteLine("Init multi part upload failed, {0}", ex.Message);
}
// Cancel the multipart upload.
try
{
    var request = new AbortMultipartUploadRequest(bucketName, objectName, uploadId);
    client.AbortMultipartUpload(request);
    Console.WriteLine("Abort multi part succeeded， {0}", uploadId);
}
catch (Exception ex)
{
    Console.WriteLine("Abort multi part failed, {0}", ex.Message);
}
```

## List uploaded parts { .section}

For the complete code of listing uploaded parts, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/MultipartUploadSample.cs).

Run the following code to list uploaded parts:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
var uploadId = "<yourUploadId>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    PartListing listPartsResult = null;
    var nextMarker = 0;
    do 
    {
        var listPartsRequest = new ListPartsRequest(bucketName, objectName, uploadId) 
        {
            PartNumberMarker = nextMarker,
        };
        // List uploaded parts.
        listPartsResult = client.ListParts(listPartsRequest);
        Console.WriteLine("List parts succeeded");
        // Upload each part simultaneously until all parts are uploaded.
        var parts = listPartsResult.Parts;
        foreach (var part in parts)
        {
            Console.WriteLine("partNumber: {0}, ETag: {1}, Size: {2}", part.PartNumber, part.ETag, part.Size);
        }
        nextMarker = listPartsResult.NextPartNumberMarker;
    } while (listPartsResult.IsTruncated);
}
catch (Exception ex)
{
    Console.WriteLine("List parts failed, {0}", ex.Message);
}
```

## List multipart upload events { .section}

Call ossClient.listMultipartUploads to list all ongoing part upload events \(events that have been initiated but not completed or have been canceled\). You can configure the following parameters:

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|prefix|Specifies the prefix that must be included in the returned object name. Note that if you use a prefix for query, the returned object name will contain the prefix.|ListMultipartUploadsRequest.setPrefix\(String prefix\)|
|delimiter|Specifies a delimiter of a forward slash \(/\) used to group object names. The object between the specified prefix and the first occurrence of a delimiter of a forward slash \(/\) is commonPrefixes.|ListMultipartUploadsRequest.setDelimiter\(String delimiter\)|
|maxUploads|Specifies the maximum number of part upload events. The maximum value \(also default value\) you can set is 1,000.|ListMultipartUploadsRequest.setMaxUploads\(Integer maxUploads\)|
|keyMarker|Lists all part upload events with the object whose names start with a letter that comes after the keyMarker value in the alphabetical order. You can use this parameter with the uploadIdMarker parameter to specify the initial position for the specified returned result.|ListMultipartUploadsRequest.setKeyMarker\(String keyMarker\)|
|uploadIdMarker|You can use this parameter with the keyMarker parameter to specify the initial position for the specified returned result. If you do not configure keyMarker, the uploadIdMarker parameter is invalid. If you configure keyMarker, the query result contains:-   All objects whose names start with a letter that comes after the keyMarker value.
-   All objects whose names start with a letter that is the same as the keyMarker value in the alphabetical order and the value of uploadId greater than that of uploadIdMarker.

|ListMultipartUploadsRequest.setUploadIdMarker\(String uploadIdMarker\)|

For the complete code of listing multiplart upload events, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/MultipartUploadSample.cs).

Run the following code to list all multipart upload events:

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
    MultipartUploadListing multipartUploadListing = null;
    var nextMarker = string.Empty;
    do
    {
        // List multipart upload events.
        var request = new ListMultipartUploadsRequest(bucketName)
        {
            KeyMarker = nextMarker,
        };
        multipartUploadListing = client.ListMultipartUploads(request);
        Console.WriteLine("List multi part succeeded");
        // List the information about multipart upload events.
        foreach (var mu in multipartUploadListing.MultipartUploads)
        {
            Console.WriteLine("Key: {0},  UploadId: {1}", mu.Key, mu.UploadId);
        }
        // If the returned result shows that the value of isTruncated is false, the values of nextKeyMarker and nextUploadIdMarker are returned and used as the initial position for the next object reading.
        nextMarker = multipartUploadListing.NextKeyMarker;
    } while (multipartUploadListing.IsTruncated);
}
catch (Exception ex)
{
    Console.WriteLine("List multi part uploads failed, {0}", ex.Message);
}
```

## Specify the prefix and maximum number of returned results { .section}

Run the following code to perform multipart upload with a specified prefix and maximum number of returned results:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
// Definition prefix.
var prefix = "<yourObjectPrefix>";
// Define maximum return bar number to 100.
var maxUploads = 100;
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    MultipartUploadListing multipartUploadListing = null;
    var nextMarker = string.Empty;
    do
    {
        // List slice upload events. By default, list 1000 slices.
        var request = new ListMultipartUploadsRequest(bucketName)
        {
            KeyMarker = nextMarker,
            // Specified prefix.
            Prefix = prefix,
            // Specifies the maximum number of bars returned.
            MaxUploads = maxUploads,
        };
        multipartUploadListing = client.ListMultipartUploads(request);
        Console.WriteLine("List multi part succeeded");
        // List the information about multipart upload events.
        foreach (var mu in multipartUploadListing.MultipartUploads)
        {
            Console.WriteLine("Key: {0},  UploadId: {1}", mu.Key, mu.UploadId);
        }
        nextMarker = multipartUploadListing.NextKeyMarker;
    } while (multipartUploadListing.IsTruncated);
}
catch (Exception ex)
{
    Console.WriteLine("List multi part uploads failed, {0}", ex.Message);
}
```

