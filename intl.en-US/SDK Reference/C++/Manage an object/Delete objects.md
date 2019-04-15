# Delete objects {#concept_flf_frl_ngb .concept}

This topic describes how to delete objects.

**Warning:** Use caution when deleting objects because deleted objects cannot be recovered.

## Delete an object {#section_tlx_rkz_xgb .section}

Use the following code to delete an object:

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

## Delete multiple objects {#section_drh_fmz_xgb .section}

Use the following code to delete multiple objects:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
     /*Initialize the OSS account information.*/
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    DeleteObjectsRequest request(BucketName);
    /* Add the names of the objects you want to delete. */
    request.addKey(ObjectName);

    /* Delete the objects. */
    auto outcome = client.DeleteObjects(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "DeleteObjects fail" <<
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

