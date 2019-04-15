# Quick start {#concept_zb4_r4r_pgb .concept}

This topic describes how to use the OSS C++ SDK to complete routine operations such as bucket creation, file uploads, and object downloads.

## Create a bucket {#section_ojm_13x_pgb .section}

A bucket is a global namespace in OSS. It is similar to a data container that stores files. Use the following code to create a bucket:

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
  
    if (! outcome.isSuccess()) {
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

For more information about the bucket naming rules, see the "Naming conventions" section in [Basic concepts](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#). For more information about how to create a bucket, see [Buckets](reseller.en-US/SDK Reference/C++/Buckets.md#).

For more information about how to obtain endpoint information, see [Regions and endpoints](../../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).

## Upload a file {#section_n3t_23x_pgb .section}

Use the following code to upload a file to OSS:

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
    std::string ObjectName = "yourObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Upload the file. */
    auto outcome = client.PutObject(BucketName, ObjectName,"yourLocalFilename");

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "PutObject fail" <<
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

## Download an object {#section_bn2_33x_pgb .section}

Use the following code to download an object to a local file:

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
    std::string ObjectName = "yourObjectName";
    /* Set the name of the object you want to download. */
    std::string FileNametoSave = "yourFileName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    /* Download the object to the local file. */
    GetObjectRequest request(BucketName, ObjectName);
    request.setResponseStreamFactory([=]() {return std::make_shared<std::fstream>(FileNametoSave, std::ios_base::out | std::ios_base::in | std::ios_base::trunc| std::ios_base::binary); });

    auto outcome = client.GetObject(request);

    if (outcome.isSuccess()) {    
        std::cout << "GetObjectToFile success, ContentLength is" << outcome.result(). Metadata(). ContentLength() << std::endl;
    }
    else {
        /* Handle exceptions. */
        std::cout << "GetObjectToFile fail" <<
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

## List objects {#section_pjv_43x_pgb .section}

Use the following code to list objects in the specified bucket: You can list a maximum of 100 objects by default.

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
  
    /* List the objects in the specified bucket. */
    ListObjectsRequest request(BucketName);
    auto outcome = client.ListObjects(request);

    if (! outcome.isSuccess()) {    
        /* Handle exceptions. */
        std::cout << "ListObjects fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;  
    }
    else {
        for (const auto& object : outcome.result(). ObjectSummarys()) {
            std::cout << "object"<<
            ",name:" << object.Key() <<
            ",size:" << object.Size() <<
            ",last modified time:" << object.LastModified() << std::endl;
        }      
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Delete an object {#section_j4t_l3x_pgb .section}

Use the following code to delete the specified object:

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
    std::string ObjectName = "yourObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    DeleteObjectRequest request(BucketName, ObjectName);

    /* Delete the object. */
    auto outcome = client.DeleteObject(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "DeleteObject fail" <<
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

For more information, see the "[Delete an object](reseller.en-US/SDK Reference/C++/Manage an object/Delete objects.md#)" section in Manage files.

