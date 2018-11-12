# Manage ACL for an object {#concept_91919_zh .concept}

The following table describes the permissions included in the Access Control List \(ACL\) for an object.

|Permission|Description|Value|
|:---------|:----------|:----|
|default|The ACL of an object is the same with that of its bucket.|CannedAccessControlList.Default|
|Private|Only the object owner and authorized users can read and write the object.|CannedAccessControlList.Private|
|Public read|Only the object owner and authorized users can read and write the object. Other users can only read the object. Authorize this permission with caution.|CannedAccessControlList.PublicRead|
|Public read-write|All users can read and write the object. Authorize this permission with caution.|CannedAccessControlList.PublicReadWrite|

The ACL of objects take precedence over that of buckets. For example, if the ACL of a bucket is private, while the object ACL is public read-write, all users can read and write the object. If an object is not configured with an ACL, its ACL is the same as that of its bucket by default.

For the complete code of configuring an ACL for an object, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/ModifyObjectMetaSample.cs). For the complete code of obtaining the ACL for an object, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob参考上面的示例代码/master/samples/Samples/GetObjectAclSample.cs).

You can run the following code to configure an ACL for an object and obtain the ACL for an object:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;

var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";

// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
// Configure an ACL for an object.
try
{
    // Use SetObjectAcl to configure an ACL for an object.
    client.SetObjectAcl(bucketName, objectName, CannedAccessControlList.PublicRead);
    Console.WriteLine("Set Object:{0} ACL succeeded ", objectName);
}
catch (Exception ex)
{
    Console.WriteLine("Set Object ACL failed with error info: {0}", ex.Message);
}
// Obtain the ACL for an object.
try
{
    // Use GetObjectAcl to obtain the ACL for an object.
    var result = client.GetObjectAcl(bucketName, objectName);
    Console.WriteLine("Get Object ACL succeeded, Id: {0}  ACL: {1}",
        result.Owner.Id, result.ACL.ToString());
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID: {2}\tHostID: {3}",
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

For details about object ACL, see [Access control](../../../../reseller.en-US/Developer Guide/Access and control/Access control.md#).

