# Range download {#concept_90282_zh .concept}

If you only need part of the data in an object, you can use range download to download the specified content.

Use the following code for range download:

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
    std::string ObjectName = "yourObectName ";

     /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret,  conf );
  
    /* Obtain the object. */
    GetObjectRequest request(BucketName,  ObjectName);
    /* Set the download range. */
    request.setRange(0, 1);
    auto outcome = client.GetObject(request);

    if (! outcome.isSuccess ()) {    
        /* Handle exceptions. */
        std::cout << "getObject fail" <<
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

