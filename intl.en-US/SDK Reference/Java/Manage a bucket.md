# Manage a bucket {#concept_32012_zh .concept}

A bucket serves as a container that stores objects. Objects belong to a bucket.

## Create a bucket {#section_zgm_l1b_kfb .section}

Run the following code to create a bucket:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Create a bucket.
String bucketName = "<yourBucketName>";
// The default storage class and ACL of the created bucket are set to standard and private read respectively.
ossClient.createBucket(bucketName);

// Close your OSSClient.
ossClient.shutdown();

```

For more information about bucket naming rules, see [naming conventions](../../../../reseller.en-US/Developer Guide/Basic concepts.md#section_yxy_jmt_tdb) in [Basic concepts](../../../../reseller.en-US/Developer Guide/Basic concepts.md#).

You can specify the ACL and the storage class when you create a bucket, as shown in [Bucket-level permissions](../../../../reseller.en-US/Developer Guide/Manage buckets/Set bucket read and write permissions.md#) and [Storage class](../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#). To create Infrequent Access or archive buckets, use Java SDK versions 2.6.0 or later. The sample code is as follows:

```language-java
CreateBucketRequest createBucketRequest = new CreateBucketRequest(bucketName);
// Set the ACL to public read for a bucket. The default value is Private.
createBucketRequest.setCannedACL(CannedAccessControlList.PublicRead);
// Set the storage class to Infrequent Access for a bucket. The default value is Standard.
createBucketRequest.setStorageClass(StorageClass.IA);
ossClient.createBucket(createBucketRequest);

```

## List buckets { .section}

Buckets are listed alphabetically. You can list all buckets or buckets that meet the specified conditions. To list Infrequent Access or archive buckets, use Java SDK versions 2.6.0 or later.

-   List all buckets

    Run the following code to list all buckets:

    ```language-java
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // List buckets.
    List<Bucket> buckets = ossClient.listBuckets();
    for (Bucket bucket : buckets) {
        System.out.println(" - " + bucket.getName());
    }
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

-   List buckets with a specified prefix

    Run the following code to list buckets with a specified prefix:

    ```language-java
    ListBucketsRequest listBucketsRequest = new ListBucketsRequest();
    // List buckets by a specified prefix. If no prefix is specified, list all buckets.
    listBucketsRequest.setPrefix("<yourBucketPrefix>");
    BucketList bucketList = ossClient.listBuckets(listBucketsRequest);
    for (Bucket bucket : bucketList.getBucketList()) {
        System.out.println(" - " + bucket.getName());
    }
    
    ```

-   List buckets by marker

    The value of the marker parameter indicates the name of a bucket. Run the following code to list buckets after the specified bucket \(the value of marker\):

    ```language-java
    ListBucketsRequest listBucketsRequest = new ListBucketsRequest();
    // List buckets after the specified marker. If the marker is not specified, list all buckets.
    listBucketsRequest.setMarker("<yourBucketMarker>");
    BucketList bucketList = ossClient.listBuckets(listBucketsRequest);
    for (Bucket bucket : bucketList.getBucketList()) {
        System.out.println(" - " + bucket.getName());
    }
    
    ```

-   List a specified number of buckets

    Run the following code to list a specified number of buckets:

    ```language-java
    ListBucketsRequest listBucketsRequest = new ListBucketsRequest();
    // Set the number of buckets that can be listed to 500. The default value is 100. The maximum value is 1,000.
    listBucketsRequest.setMaxKeys(500);
    BucketList bucketList = ossClient.listBuckets(listBucketsRequest);
    for (Bucket bucket : bucketList.getBucketList()) {
        System.out.println(" - " + bucket.getName());
    }
    
    ```


## Determine whether a bucket exists { .section}

Run the following code to determine whether a specified bucket exists:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

boolean exists = ossClient.doesBucketExist("<yourBucketName>");

// Close your OSSClient.
ossClient.shutdown();

```

## Configure an ACL for a bucket { .section}

The ACL of a bucket includes the following permissions.

|Permission|Description|Value in Java SDK|
|:---------|:----------|:----------------|
|Private|The bucket owner and the authorized users can read and write objects in the bucket. Other users cannot perform any operation on the objects.|CannedAccessControlList.Private|
|Public read|The bucket owner and the authorized users can read and write objects in the bucket. Other users can only read the objects in the bucket. Authorize this permission with caution.|CannedAccessControlList.PublicRead|
|Public read-write|All users can read and write objects in the bucket. Authorize this permission with caution.|CannedAccessControlList.PublicReadWrite|

Run the following code to configure the ACL for a bucket:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Set the ACL for a bucket to private read and write.
ossClient.setBucketAcl("<yourBucketName>", CannedAccessControlList.Private);

// Close your OSSClient.
ossClient.shutdown();

```

## Obtain the ACL for a bucket { .section}

Run the following code to obtain the ACL for a bucket:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
// Obtain the access ACL for the bucket
AccessControlList acl = ossClient.getBucketAcl("<yourBucketName>");
System.out.println(acl.toString());

// Close your OSSClient.
ossClient.shutdown();

```

## Obtain the region of a bucket { .section}

Run the following code to obtain the region for a bucket:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

String location = ossClient.getBucketLocation("<yourBucketName>");
System.out.println(location);

// Close your OSSClient.
ossClient.shutdown();

```

For more information about regions, see [Region](../../../../reseller.en-US/Developer Guide/Basic concepts.md#section_s3j_nmt_tdb) in [Basic concepts](../../../../reseller.en-US/Developer Guide/Basic concepts.md#).

## Obtain bucket information { .section}

Run the following code to obtain bucket information:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We strongly recommend that you create a RAM account and use it for API access and daily O&M. Log on to https://ram.console.aliyun.com to create a RAM account.
String accessKeyId = "<accessKeyId>";
String accessKeySecret = "<accessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

Bucket information includes regions (region or location), creation dates (CreationDate), owners (Owner), and permissions (Grants).
BucketInfo info = ossClient.getBucketInfo("<yourBucketName>");
// Obtain region information.
info.getBucket().getLocation();
// Obtain the creation date.
info.getBucket().getCreationDate();
// Obtain owner information.
info.getBucket().getOwner();
// Obtain permission information.
info.getGrants();

// Close your OSSClient.
ossClient.shutdown();

```

## Delete a bucket { .section}

Before a bucket is deleted, ensure that all objects in the bucket and fragments that are generated from multipart upload are deleted.

**Note:** To delete the fragments that are generated from multipart upload, use [Bucket.ListMultipartUploads](../../../../reseller.en-US/API Reference/Multipart upload operations/ListMultipartUploads.md#) to list all fragments, and then use [Bucket.AbortMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/AbortMultipartUpload.md#) to delete the fragments.

Run the following code to delete a bucket:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

/// Delete the bucket.
ossClient.deleteBucket("<yourBucketName>");

// Close your OSSClient.
ossClient.shutdown();

```

