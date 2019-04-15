# Hotlinking protection {#concept_89700_zh .concept}

This topic describes how to use hotlinking protection.

为了防止您在OSS上的数据被其他人盗链而产生额外费用，您可以设置防盗链功能，包括以下参数：

-   Referer白名单。仅允许指定的域名访问OSS资源。
-   是否允许空Referer。如果不允许空Referer，则只有HTTP或HTTPS header中包含Referer字段的请求才能访问OSS资源。

For more information about hotlinking protection, see the "[Hotlinking protection settings](../../../../../reseller.en-US/Developer Guide/Buckets/Anti-leech settings.md#)" section in OSS Developer Guide.

## Configure hotlinking protection {#section_hgk_3kz_kfb .section}

Use the following code to configure hotlinking protection:

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
 
    /* Configure hotlinking protection. */
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

## Obtain hotlinking protection information {#section_my3_x3l_ngb .section}

Use the following code to obtain hotlinking protection information:

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
 

    /* Obtain hotlinking protection information. */
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

## Clear hotlinking protection configurations {#section_ny3_x3l_ngb .section}

Use the following code to clear hotlinking protection configurations:

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

