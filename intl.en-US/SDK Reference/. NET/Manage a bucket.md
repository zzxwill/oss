# Manage a bucket {#concept_32089_zh .concept}

A bucket serves as a container that stores objects. Objects belong to a bucket.

## Create a bucket {#section_s5m_v12_lfb .section}

For the complete code of creating a bucket, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/CreateBucketSample.cs).

You can run the following code to create a bucket:

```language-csharp
using Aliyun.OSS;

// Initialize an OSSClient.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);

// Create a new bucket.
public void CreateBucket(string bucketName)
{
    try
    {
        // Create a bucket. The bucketName is globally unique.
        client.CreateBucket(bucketName);
        Console.WriteLine("Create bucket succeeded");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Create bucket failed. {0}", ex.Message);
    }
}

```

## List buckets { .section}

For the complete code of listing buckets, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/ListBucketsSample.cs).

You can run the following code to list all buckets under an account:

```language-csharp
using Aliyun.OSS;

// Initialize an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);

// List the information about all buckets under an account.
public void ListBuckets()
{
    try
    {
        var buckets = client.ListBuckets();
        
        Console.WriteLine("List bucket succeeded");
        foreach (var bucket in buckets)
        {
	        Console.WriteLine("Bucket name：{0}，Location：{1}，Owner：{2}", bucket.Name, bucket.Location, bucket.Owner);
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine("List bucket failed. {0}", ex.Message);
    }
}

```

## Determine whether a bucket exists { .section}

For the complete code of determining whether a bucket exists, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/DoesBucketExistSample.cs).

You can run the following code to determine whether a bucket exists:

```language-csharp
using Aliyun.OSS;

//Initialize an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);

// Determine whether a bucket exists.
public void DoesBucketExist(string bucketName)
{
    try
    {
        var exist = client.DoesBucketExist(bucketName);

        Console.WriteLine("Check object Exist succeeded");
        Console.WriteLine("exist ? {0}", exist);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Check object Exist failed. {0}", ex.Message);
    }
}

```

## Configure an ACL for a bucket { .section}

For the complete code of configuring ACL, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/SetBucketAclSample.cs).

The ACL of a bucket includes the following permissions.

|Permission|Description|Value|
|:---------|:----------|:----|
|Private|The bucket owner and the authorized users can read and write objects in the bucket. Other users cannot perform any operation on the objects.|oss.ACLPrivate|
|Public read|The bucket owner and the authorized users can read and write objects in the bucket. Other users can only read the objects in the bucket. Authorize this permission with caution.|oss.ACLPublicRead|
|Public read-write|All users can read and write objects in the bucket. Authorize this permission with caution.|oss.ACLPublicReadWrite|

You can run the following code to configure an ACL for a bucket:

```language-csharp
using Aliyun.OSS;

// Initialize an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);

// Configure an access ACL for the bucket
public void SetBucketAcl(string buckteName)
{
    try
    {
        // Set the ACL of the bucket to public read.
        client.SetBucketAcl(bucketName, CannedAccessControlList.PublicRead);
        Console.WriteLine("Set bucket ACL succeeded");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Set bucket ACL failed. {0}", ex.Message);
    }
}

```

## Obtain the ACL for a bucket { .section}

For the complete code of obtaining the ACL for a bucket, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/GetBucketAclSample.cs).

You can run the following code to obtain the ACL for a bucket:

```language-csharp
using Aliyun.OSS;

// Initialize an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);

// Obtain the ACL for the bucket.
public void GetBucketAcl(string bucketName)
{
    try
    {
        string bucketName = "your-bucket";
        var acl = client.GetBucketAcl(bucketName);

        Console.WriteLine("Get bucket ACL success");
        foreach (var grant in acl.Grants)
        {
             Console.WriteLine ("The bucket ACL has been obtained successfully. Current ACL: {0}", grant.Permission.ToString());
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine("Get bucket ACL failed. {0}", ex.Message);
    }
}

```

## Delete a bucket { .section}

For the complete code of deleting a bucket, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/DeleteBucketSample.cs).

Before a bucket is deleted, ensure that all objects in the bucket and fragments that are generated from multipart upload are deleted.

**Note:** To delete the fragments that are generated from multipart upload, use [Bucket.ListMultipartUploads](../../../../reseller.en-US/API Reference/Multipart upload operations/ListMultipartUploads.md#) to list all fragments, and then use [Bucket.AbortMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/AbortMultipartUpload.md#) to delete the fragments.

You can run the following code to delete a bucket:

```language-csharp
using Aliyun.OSS;

// Initialize an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);

// Delete the bucket.
public void DeleteBucket(string bucketName)
{
    try
    {
        client.DeleteBucket(bucketName);
        
        Console.WriteLine("Delete bucket succeeded");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Delete bucket failed. {0}", ex.Message);
    }
}

```

