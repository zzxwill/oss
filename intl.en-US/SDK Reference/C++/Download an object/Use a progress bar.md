# Use a progress bar {#concept_90284_zh .concept}

Progress bars are used to indicate the progress of an upload or download.

For the complete code of download progress bars, see [GitHub](https://github.com/aliyun/aliyun-oss-cpp-sdk/blob/master/sample/src/object/ObjectSample.cc#L414).

Use the following code to view the progress information:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

void ProgressCallback(size_t increment, int64_t transfered, int64_t total, void* userData)
{
    std::cout << "ProgressCallback[" << userData << "] => " <<
                 increment <<" ," << transfered << "," << total << std::endl;
}

int main(void )
{
     /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName" ;

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeyS ecre t, conf);
  
    /* Obtain the object and its download progress information. */
    GetObjectRequest request(BucketName, ObjectName);
    TransferProgress progressCallback = { ProgressCallback , nullptr };
    request.setTransferProgress(progressCallback);

    auto outcome = client.GetObject(request);

    if (! outcome.isSuccess()) {    
        /* Handle exceptions. */
        std::cout << "getObject fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk ();
         return -1;  
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

