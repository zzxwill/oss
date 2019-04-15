# Append upload {#concept_90215_zh .concept}

This topic describes how to append data from a local file.

Use the following code to append data from a local file:

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

    auto meta = ObjectMetaData();
    meta.setContentType("text/plain");

    // If the object is appended for the first time, the append position is 0, and the returned value is the position for the next append. The position for the next append is the length of the current object. */
    std::shared_ptr<std::iostream> content1 = std::make_shared<std::stringstream>();
    *content1 <<"Thank you for using Aliyun Object Storage Service!" ;
    AppendObjectRequest request(BucketName, ObjectName, content1, meta);
    request.setPosition(0L);

    /* Start the first append. */
    auto result = client.AppendObject(request);
  
    if (! result.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "AppendObject fail" <<
        ",code:" << result.error(). Code() <<
        ",message:" << result.error(). Message() <<
        ",requestId:" << result.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    std::shared_ptr<std::iostream> content2 = std::make_shared<std::stringstream>();
    *content2 <<"Thank you for using Aliyun Object Storage Service!" ;
    auto position = result.result(). Length();
    AppendObjectRequest appendObjectRequest(BucketName, ObjectName, content2);
    appendObjectRequest.setPosition(position);

    /* Start the second append. */
    auto outcome = client.AppendObject(appendObjectRequest);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "AppendObject fail" <<
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

If the object already exists, one of the following situations occurs:

-   If the object is not appendable, ObjectNotAppendable is thrown.
-   If the object is appendable, PutObject is used to upload the object, the object overwrites the existing object, and the object type becomes Normal Object. After the HeadObject operation is performed, OSS returns x-oss-object-type.

