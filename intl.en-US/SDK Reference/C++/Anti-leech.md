# Anti-leech {#concept_89700_zh .concept}

This topic describes how to use anti-leech.

To prevent your data on OSS from being leeched, OSS supports anti-leeching through the referer field settings in the HTTP header, including the following parameters:

-   Referer whitelist: Used to allow access only for specified domains to OSS data.
-   Empty referer: Determines whether the referer can be empty. If it is not allowed, only requests with the referer filed in their HTTP or HTTPS headers can access OSS data.

For more information about anti-leech, see the "[Anti-leeching settings](#)" section in the OSS Developer Guide.

## Configure the referer whitelist {#section_hgk_3kz_kfb .section}

Run the following code to configure the referer whitelist:

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
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret,  conf) ;
 
    /* Configure the referer whitelist. */
    SetBucketRefererRequest request(BucketName);
    request.addReferer("http://www.referersample.com");
    request.addReferer("https://www.referersample.com");
    request.addReferer("https://www.?.referersample.com");
    request.addReferer("https://www. *.cn");
    request.setAllowEmptyReferer(false);

    auto outcome = client.SetBucketReferer(request);
  
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "SetBucketReferer fail" <<
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

## Obtain a referer whitelist {#section_my3_x3l_ngb .section}

Run the following code to obtain a referer whitelist:

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
 

    /* Obtain the referer list for a bucket. */
    GetBucketRefererRequest request(BucketName);
    auto outcome = client.GetBucketReferer(request);

    if (outcome.isSuccess()) {
        std::cout << " GetBucketReferer success, AllowEmptyReferer: " << outcome.result(). AllowEmptyReferer() <<
        " ,Referer size: " << outcome.result(). RefererList().size() << std::endl;
    }
    else { 
        /* Handle exceptions. */
        std::cout << "GetBucketReferer fail" <<
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

## Clear a referer whitelist {#section_ny3_x3l_ngb .section}

Run the following code to clear a referer whitelist:

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
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret,  conf);
 
    /* You cannot clear a referer whitelist directly. To clear a referer whitelist, you must create a rule that allows an empty referer field and replace the original rule with the new rule. */
    SetBucketRefererRequest request(BucketName);
    request.setAllowEmptyReferer(true);

    auto outcome = client.SetBucketReferer(request);
  
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "CleanBucketReferer fail" <<
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

