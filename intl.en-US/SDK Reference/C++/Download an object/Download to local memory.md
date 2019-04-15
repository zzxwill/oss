# Download to local memory {#concept_90280_zh .concept}

This topic describes how to download an object to local memory.

Use the following code to download an object to local memory:

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
    std::string content =  "";

     /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret,  conf);
   
    /* Download the object to local memory. */
    GetObjectRequest request(BucketName, ObjectName);
    auto outcome = client.GetObject(request);
    *outcome.result(). Content() >> content;
    if (outcome.isSuccess()) {    
        std::cout << "getObjectToBuffer" << " success, Content-Length:" << outcome.result(). Metadata(). ContentLength() << ",content:" << content << std::endl;
     }
 
    else {
        /* Handle exceptions. */
        std::cout << "getObjectToBuffer fail" <<
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

