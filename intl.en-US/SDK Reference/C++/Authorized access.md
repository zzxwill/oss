# Authorized access {#concept_32139_zh .concept}

This topic describes how to authorize access to OSS.

## Use STS for temporary access authorization {#section_nyk_bzc_kfb .section}

You can use Alibaba Cloud STS to authorize temporary access to OSS. STS is a Web service that provides a temporary access token to a cloud computing user. You can use STS to grant a third-party application or a RAM user \(whose user ID is managed by yourself\) an access credential with a customized validity period and permissions. For more information about STS, see [STS Introduction](../../../../../reseller.en-US/API Reference (STS)/Introduction.md#).

STS benefits:

-   You do not need to expose your long-term key \(AccessKey\) to a third-party application. You need only to generate an access token and send the access token to the third-party application. You can customize the access permissions and validity period of this token.
-   The access token automatically becomes invalid after expiration.

For more information about the process of access to OSS with STS, see the "[Identity authentication](../../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#)" section in OSS Developer Guide.

Use the following code to create a signature request with STS:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string SecurityToken  = "securityToken";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, SecurityToken, conf)

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Use a signed URL to authorize temporary access {#section_pkd_54l_ngb .section}

You can provide a signed URL to a visitor for temporary access. When you generate a signature for a URL, you can specify the valid period for the URL. After the valid period expires, access from visitors is restricted.

-   Use a signed URL to upload an object

    Use the following code to generate a signed URL to upload an object:

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
        std::string PutobjectUrlName = "yourPutobjectUrlName" ;
     
         /* Initialize network resources. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
      
        /* Specify the valid period for the URL.*/
        std::time_t t = std::time(nullptr) + 1200;
        /* Generate the signed URL for uploading an object. */
        auto genOutcome = client.GeneratePresignedUrl(BucketName, PutobjectUrlName, t, Http::Put);
        if (genOutcome.isSuccess()) {
            std::cout << "GeneratePresignedUrl success, Gen url:" << genOutcome.result().c_str() << std::endl;
        }
        else {
            /* Handle exceptions. */
            std::cout << "GeneratePresignedUrl fail" <<
            ",code:" << genOutcome.error(). Code() <<
            ",message:" << genOutcome.error(). Message() <<
            ",requestId:" << genOutcome.error(). RequestId() << std::endl;
            ShutdownSdk();
            return -1;
        }
      
        std::shared_ptr<std::iostream> content = std::make_shared<std::stringstream>();
        *content << "test cpp sdk";
    
        /* Use the URL to upload the object. */
        auto outcome = client.PutObjectByUrl(genOutcome.result(), content);
    
        if (! outcome.isSuccess()) {
            /* Handle exceptions. */
            std::cout << "PutObjectByUrl fail" <<
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

-   Use a signed URL to download an object

    Use the following code to generate a signed URL to download an object:

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
        std::string GetobjectUrlName = "yourGetobjectUrlName";
    
        /* Initialize network resources. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
      
        /* Specify the valid period for the URL.*/
        std::time_t t = std::time(nullptr) + 1200;
        /* Generate the URL to download the object. */
        auto genOutcome = client.GeneratePresignedUrl(BucketName, GetobjectUrlName, t, Http::Get);
        if (genOutcome.isSuccess()) {
            std::cout << "GeneratePresignedUrl success, Gen url:" << genOutcome.result().c_str() << std::endl;
        }
        else {
            /* Handle exceptions. */
            std::cout << "GeneratePresignedUrl fail" <<
            ",code:" << genOutcome.error(). Code() <<
            ",message:" << genOutcome.error(). Message() <<
            ",requestId:" << genOutcome.error(). RequestId() << std::endl;
            ShutdownSdk();
            return -1;
        }
      
        /* Use the URL to download the object. */
        auto outcome = client.GetObjectByUrl(genOutcome.result());
    
        if (! outcome.isSuccess()) {
            /* Handle exceptions. */
            std::cout << "GetObjectByUrl fail" <<
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


