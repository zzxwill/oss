# Access logging {#concept_89684_zh .concept}

You can enable access logging to record access to a bucket in a log. The log is stored in a specified bucket.

The log format is as follows:

 `<TargetPrefix><SourceBucket>-YYYY-mm-DD-HH-MM-SS-UniqueString` 

For more information about access logging, see the "[Configure access logging](../../../../../reseller.en-US/Developer Guide/Manage logs/访问日志存储.md#)" section in OSS Developer Guide.

## Enable access logging {#section_sxn_wjz_kfb .section}

Use the following code to enable access logging for a bucket:

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
    std::string TargetBucketName = "yourTargetBucketName";
    std::string TargetPrefix  ="yourTargetPrefix";
 
    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret ,  conf);  
 
    /* Enable access logging. */
    SetBucketLoggingRequest request(BucketName, TargetBucketName, TargetPrefix);
    auto outcome = client.SetBucketLogging(request);
  
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "SetBucketLogging fail" <<
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

## View access logging configurations {#section_atq_bnl_ngb .section}

Use the following code to view the access logging configurations of a bucket:

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

    /* View the access logging configurations. */
    GetBucketLoggingRequest request(BucketName);
    auto outcome = client.GetBucketLogging(request);

    if (outcome.isSuccess()) {
        std::cout <<" GetBucketLogging success, TargetBucket: " << outcome.result(). TargetBucket() << 
        ",TargetPrefix: " << outcome.result(). TargetPrefix() << std::endl;
    }
    else { 
        /* Handle exceptions. */
        std::cout << "GetBucketLogging fail" <<
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

## Disable access logging {#section_btq_bnl_ngb .section}

Use the following code to disable access logging for a bucket:

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
 
    /* Disable access logging. */
    DeleteBucketLoggingRequest request(BucketName);
    auto outcome = client.DeleteBucketLogging(request);
  
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "DeleteBucketLogging fail" <<
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

