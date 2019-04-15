# List objects {#concept_jds_yrl_ngb .concept}

This topic describes how to list the objects stored in a bucket.

Objects are listed alphabetically. You can use OssClient.ListObjects to list the objects in a bucket. ListObjects supports three types of parameter formats:

-   ListObjectOutcome ListObjects\(const std::string& bucket\) const: lists objects in a bucket. A maximum of 100 objects can be listed.
-   ListObjectOutcome ListObjects\(const std::string& bucket, const std::string& prefix\) const: lists objects with the specified prefix in a bucket. A maximum of 100 objects can be listed.
-   ListObjectOutcome ListObjects\(const ListObjectsRequest& request\) const: provides multiple filtering functions to flexibly query objects.

The following table describes parameters for object listing.

|Name|Description|
|:---|:----------|
|delimiter|The delimiter used to group objects. CommonPrefixes specify a set of substrings that start with a specified prefix and end with the first specified delimiter.|
|prefix|The prefix that must be included in the returned objects.|
|maxKeys|The maximum number of objects that can be listed. The default value is 100. The maximum value is 1,000.|
|marker|The initial object in the list.|

## Simple list {#section_xcv_k35_xgb .section}

Use the following code to list objects in the specified bucket. A maximum of 100 objects can be listed by default.

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
  
    /* List the objects. */
    ListObjectsRequest request(BucketName);
    auto outcome = client.ListObjects(request);

    if (! outcome.isSucces s())  {    
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
            ",lastmodify time:" << object.LastModified() << std:: endl;
         }      
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## List a specified number of objects {#section_f5j_2h5_xgb .section}

Use the following code to list a specified number of objects:

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
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf) ;
  
    /* List the objects. */
    ListObjectsRequest request(BucketName);
    /* Configure the maximum number of objects you want to list. */
    request.setMaxKeys(200);
    auto outcome = client.ListObjects(request);

    if (! outcome.isSuccess( )) {    
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
            ",lastmodify time:" << object.LastModified() << std ::endl; 
        }      
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## List the objects and subfolders of a folder {#section_wtw_nh5_xgb .section}

Use the following code to list the objects and subfolders of a folder:

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
    std::string keyPrefix = "yourkeyPrefix ";

     /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    std::string nextMarker = "";
    do {
        /* List the objects. */
        ListObjectsRequest request(BucketName);
         /* Set forward slashes (/) as the delimiter for folders. */
        request.setDelimiter("/");
        request.setPrefix(keyPrefix);
        request.setMarker(nextMarker);
        auto outcome = client.ListObjects(request);

        if (! outcome.isSuccess ()) {    
            /* Handle exceptions. */
            std::cout << "ListObjects fail" <<
            ",code:" << outcome.error(). Code() <<
            ",message:" << outcome.error(). Message() <<
            ",requestId:" << outcome.error(). RequestId() << std::endl;
            break;
        }  
        for (const auto& object : outcome.result(). ObjectSummarys()) {
            std::cout << "object"<<
            ",name:" << object.Key() <<
            ",size:" << object.Size() <<
            ",lastmodify time:" << object.LastModified() << std::endl;
        }
        for (const auto& commonPrefix : outcome.result(). CommonPrefixes()) {
            std::cout << "commonPrefix" << ",name:" << commonPrefix << std::endl;
        }
        nextMarker = outcome.result(). NextMarker();
    } while (outcome.result(). IsTruncated());
  
    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## List objects by prefix {#section_zfj_rh5_xgb .section}

Use the following code to list objects with the specified prefix:

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
    std::string keyPrefix = "yourkeyPrefix ";

     /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    std::string nextMarker = "";
    do {
        /* List the objects. */
        ListObjectsRequest request(BucketName);
        /* Specify the prefix. */
        request.setPrefix(keyPrefix);
        request.setMarker(nextMarker);
        auto outcome = client.ListObjects(request);

        if (! outcome.isSuccess ()) {    
            /* Handle exceptions. */
            std::cout << "ListObjects fail" <<
            ",code:" << outcome.error(). Code() <<
            ",message:" << outcome.error(). Message() <<
            ",requestId:" << outcome.error(). RequestId() << std::endl;
            break;
        }
        for (const auto& object : outcome.result(). ObjectSummarys()) {
            std::cout << "object"<<
            ",name:" << object.Key() <<
            ",size:" << object.Size() <<
            ",lastmodify time:" << object.LastModified() << std::endl;
        }      
        nextMarker = outcome.result(). NextMarker();
    } while (outcome.result(). IsTruncated());

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## List objects by marker {#section_mk1_vh5_xgb .section}

The marker parameter indicates the name of an object from which to start listing objects. Use the following code to specify an object from which to start listing objects:

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
    std::string YourMarker = "yourMarker ";

     /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    do {
        /* List the objects. */
        ListObjectsRequest request(BucketName);
         /* List objects after the object specified by the marker parameter. */
        request.setMarker(YourMarker);
        auto outcome = client.ListObjects(request);

        if (! outcome.isSuccess())  {    
            /* Handle exceptions. */
            std::cout << "ListObjects fail" <<
            ",code:" << outcome.error(). Code() <<
            ",message:" << outcome.error(). Message() <<
            ",requestId:" << outcome.error(). RequestId() << std::endl;
            break;  
        }
        YourMarker = outcome.result(). NextMarker();
        for (const auto& object : outcome.result(). ObjectSummarys()) {
            std::cout << "object"<<
            ",name:" << object.Key() <<
            ",size:" << object.Size() <<
            ",lastmodify time:" << object.LastModified() << std::endl;
        }      
    } while (outcome.result(). IsTruncated());

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Encode the name of an object {#section_o4c_nk5_xgb .section}

You must encode the name of an object if the name contains any of the following special characters. Only URL encoding is supported in OSS.

-   Single quotation marks \('\)
-   Double quotations marks \("\)
-   Ampersands \(&\)
-   Angle brackets \(<\>\)
-   \[DO NOT TRANSLATE\]
-   \[DO NOT TRANSLATE\]

Use the following code to encode the names of specified objects:

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
  
    std::string nextMarker = "";
    do {
        /* List the objects that need to be encoded. */
        ListObjectsRequest request(BucketName) ;
        /* Encode the names of specified objects. */
        request.setEncodingType("url");
        request.setMarker(nextMarker);
        auto outcome = client.ListObjects(request);

        if (! outcome.isSuccess())    
            /* Handle exceptions. */
            std::cout << "ListObjects fail" <<
            ",code:" << outcome.error(). Code() <<
            ",message:" << outcome.error(). Message() <<
            ",requestId:" << outcome.error(). RequestId() << std::endl;
            break; 
        }
        for (const auto& object : outcome.result(). ObjectSummarys()) {
            std::cout << "object"<<
            ",name:" << object.Key() <<
            ",size:" << object.Size() <<
            ",lastmodify time:" << object.LastModified() << std::endl;
        }      
        nextMarker = outcome.result(). NextMarker();
    } while (outcome.result(). IsTruncated());
  
    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

