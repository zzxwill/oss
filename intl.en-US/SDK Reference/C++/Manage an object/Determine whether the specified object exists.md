# Determine whether the specified object exists {#concept_fn5_trl_ngb .concept}

This topic describes how to determine whether an object exists.

Use the following code to determine whether an object exists:

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
  
    /* Determine whether the object exists. */
    auto outcome = client.DoesObjectExist(BucketName, ObjectName);

    if (outcome) {
      std::cout << "The Object exists!" << std::endl;                       
    }                              
    else {                         
      std::cout << "The Object does not exist!" << std::endl;
    }
    
    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

