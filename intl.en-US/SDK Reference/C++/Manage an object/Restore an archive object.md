# Restore an archive object {#concept_90491_zh .concept}

This topic describes how to restore an archive object.

You must restore an archive object before you can read it. Do not call RestoreObject for non-archive objects.

The state conversion process of an archive object is as follows:

-   The archive object is initially in the frozen state.
-   After you submit it for restoration, the server restores the object. The object is in the restoring state.

    **Note:** Typically, it takes about one minute to restore an archive object.

-   You can read the object after it is restored. The restored state of the object lasts one day by default. You can prolong this period to a maximum of seven days. After this period expires, the object returns to the frozen state.

Use the following code to restore an archive object:

```
#include <alibabacloud/oss/OssClient.h>
#include <thread>
#include <chrono>
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
  
    /* Restore the archive object. */
    auto outcome = client.RestoreObject(BucketName, ObjectName);

    /* Non-archive objects cannot be restored. */
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "RestoreObject fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
  
    std::string onGoingRestore("ongoing-request=\"false\"");
    
    int maxWaitTimeInSeconds = 600;
    while (maxWaitTimeInSeconds > 0)
    {
        auto meta = client.HeadObject(BucketName, ObjectName);

        std::string restoreStatus = meta.result(). HttpMetaData()["x-oss-restore"];
        std::transform(restoreStatus.begin(), restoreStatus.end(), restoreStatus.begin(), ::tolower);
        if (! restoreStatus.empty() && 
        restoreStatus.compare(0, onGoingRestore.size(), onGoingRestore)==0) {
            std::cout << " success, restore status:" << restoreStatus << std::endl;
            /* The archive object is restored.*/
            break;
        }
      
        std::cout << " info, WaitTime:" << maxWaitTimeInSeconds
        << "; restore status:" << restoreStatus << std::endl;
      
        std::this_thread::sleep_for(std::chrono::seconds(10));
        maxWaitTimeInSeconds--;     
    }
  
    if (maxWaitTimeInSeconds == 0)
    {
        std::cout << "RestoreObject fail, TimeoutException" << std::endl;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

For more information about archive storage classes, see [Introduction to storage classes](../../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#). For more information about related status codes, see [RestoreObject](../../../../../reseller.en-US/API Reference/Object operations/RestoreObject.md#) in API Reference.

