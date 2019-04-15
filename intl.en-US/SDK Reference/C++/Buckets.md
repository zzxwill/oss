# Buckets {#concept_32135_zh .concept}

A bucket serves as a container that stores objects. Objects belong to the bucket they are created in or uploaded to.

## Create a bucket {#section_ogg_55x_kfb .section}

Use the following code to create a bucket:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Specify the name, storage class, and ACL of the bucket. */
    CreateBucketRequest request(BucketName, StorageClass::IA, CannedAccessControlList::PublicReadWrite);

    /* Create the bucket. */
    auto outcome = client.CreateBucket(request);
  
    if (! outcome.isucces()) {
        /* Handle exceptions. */
        std::cout << "CreateBucket fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

For more information about the bucket naming rules, see the "Naming conventions" section in [Basic concepts](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#). You can specify the ACL and the storage class when you create a bucket, as shown in [Bucket ACLs](../../../../../reseller.en-US/Developer Guide/Buckets/Set bucket permissions (ACL).md#) and [Storage classes](../../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#).

## List buckets {#section_qjq_5pr_ngb .section}

Use the following code to list all buckets:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    /* List the buckets. */
    ListBucketsRequest request;
    auto outcome = client.ListBuckets(request);

    if (outcome.isSuccess()) {
        /* Display the bucket information. */
        std::cout <<" success, and bucket count is" << outcome.result(). Buckets().size() << std::endl;
        std::cout << "Bucket name is" << std::endl;
        for (auto result : outcome.result(). Buckets())
        {
            std::cout << result.Name() << std::endl;
        }
    }
    else {
        /* Handle exceptions. */
        std::cout << "ListBuckets fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Configure the ACL of a bucket {#section_rjq_5pr_ngb .section}

The following table describes the permissions included in the ACL of a bucket.

|Permission|Description|Value|
|:---------|:----------|:----|
|Private|Only the owner or authorized users of a bucket have read and write permissions on objects in the bucket.|OSS\_ACL\_PRIVATE|
|Public Read|Only the owner or authorized users of a bucket have read and write permissions on objects in the bucket. Other users \(including anonymous users\) have only read permissions on objects in the bucket. Use caution when using this permission.|OSS\_ACL\_PUBLIC\_READ|
|Public|All users of a bucket have read and write permissions on all objects in the bucket. Use caution when using this permission.|OSS\_ACL\_PUBLIC\_READ\_WRITE|

For more information about the ACL, see the "[Access control](../../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#)" section in OSS Developer Guide.

Use the following code to configure the ACL of a bucket:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    /* Configure the ACL of the bucket. */
    SetBucketAclRequest request(BucketName, CannedAccessControlList::Private);
    auto outcome = client.SetBucketAcl(request);

    if (outcome.isSuccess()) {    
        std::cout << " setBucketAcl to private success " << std::endl;
    }
    else {
        /* Handle exceptions. */
        std::cout << "SetBucketAcl fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Obtain the ACL of a bucket {#section_yjq_5pr_ngb .section}

Use the following code to obtain the ACL of a bucket:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    /* Obtain the ACL of the bucket. */
    GetBucketAclRequest request(BucketName);
    auto outcome = client.GetBucketAcl(request);

    if (outcome.isSuccess()) {    
        std::cout << "getBucketAcl success, acl: " << outcome.result(). Acl() << std::endl;
    }
    else {
        /* Handle exceptions. */
        std::cout << "getBucketAcl fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Obtain the region of a bucket {#section_zjq_5pr_ngb .section}

Use the following code to obtain the region where a bucket resides:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    /* Obtain the region where the bucket resides. */
    GetBucketLocationRequest request(BucketName);
    auto outcome = client.GetBucketLocation(request);

    if (outcome.isSuccess()) {    
        std::cout << "getBucketLocation success, location: " << outcome.result(). Location() << std::endl;
    }
    else {
        /* Handle exceptions. */
        std::cout << "getBucketLocation fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0；
}
```

## Determine whether a bucket exists {#section_wbm_nqr_ngb .section}

Use the following code to determine whether the specified bucket exists:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    /* Determine whether the bucket exists. */
    auto outcome = client.DoesBucketExist(BucketName);

    if (outcome) {    
        std::cout << " The Bucket exists" << std::endl;
    }
    else {
        std::cout << "The Bucket does not exist" << std::endl;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Delete a bucket {#section_bkq_5pr_ngb .section}

Before deleting a bucket, you must delete all objects in the bucket and fragments generated by multipart upload.

**Note:** To delete the fragments that are generated from multipart upload, use [ListMultipartUploads](../../../../../reseller.en-US/API Reference/Multipart upload operations/ListMultipartUploads.md#) to list all fragments, and then use [AbortMultipartUpload](../../../../../reseller.en-US/API Reference/Multipart upload operations/AbortMultipartUpload.md#) to delete the fragments.

Use the following code to delete a bucket:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Specify the name of the bucket you want to delete. */
    DeleteBucketRequest request(BucketName);

    /* Delete the bucket. */
    auto outcome = client.DeleteBucket(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "DeleteBucket fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0；
}
```

