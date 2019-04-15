# Manage a symbolic link {#concept_90490_zh .concept}

This document describes how to create a symbolic link and obtain the object content to which the symbolic link maps.

## Create a symbolic link {#section_jvn_zfz_kfb .section}

A symbolic link is a special object that maps to an object. It is similar to a shortcut used in Windows. You can use a symbolic link to customize Object Meta.

Use the following code to create a symbolic link:

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
    std::string LinkName = "yourLinkName";
 
    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

  
  
    /* Set the HTTP header. */
    auto meta = ObjectMetaData();
    meta.setContentType("text/plain");
  
    /* Set User Meta. */
    meta.UserMetaData()["meta"] = "meta-value";
  
    /* Create the symbolic link. */
    CreateSymlinkRequest request(BucketName, ObjectName, meta);
    request.SetSymlinkTarget(LinkObjectName);
    auto outcome = client.CreateSymlink(request);
  
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "CreateSymlink fail" <<
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

## Obtain object content of a symbolic link {#section_e21_jpl_ngb .section}

Use the following code to obtain the object content of a symbolic link:

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
    std::string LinkName = "yourLinkName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Obtain the object content of a symbolic link. */
    GetSymlinkRequest request(BucketName, LinkName);
    auto outcome = client.GetSymlink(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "GetSymlink fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    else { 
        std::cout << " GetSymlink success Symlink name:" << outcome.result(). SymlinkTarget() << std::endl;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

**Note:** To obtain a symbolic link, you must have the read permission on it. For more information about symbolic links, see [GetSymlink](../../../../../reseller.en-US/API Reference/Object operations/GetSymlink.md#).

