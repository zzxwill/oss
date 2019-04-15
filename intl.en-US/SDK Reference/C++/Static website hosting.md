# Static website hosting {#concept_xmv_ggl_ngb .concept}

You can set the static website hosting mode for your bucket. After the configuration takes effect, you can use the bucket domain to access this static website and be redirected to a specified index page or error page.

For more information about static website hosting, see the "[Configure static website hosting](../../../../../reseller.en-US/Developer Guide/Static website hosting/Configure static website hosting.md#)" section in OSS Developer Guide.

## Configure static website hosting {#section_w4z_5gl_ngb .section}

Use the following code to configure static website hosting:

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
   
    /* Configure static website hosting. */
    SetBucketWebsiteRequest request(BucketName);
    request.setIndexDocument("index.html");
    request.setErrorDocument("error.html");

    auto outcome = client.SetBucketWebsite(request);
  
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "SetBucketWebsite fail" <<
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

## View static website hosting configurations {#section_utm_d3l_ngb .section}

Use the following code to view static website hosting configurations:

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

    /* View static website hosting configurations. */
    GetBucketWebsiteRequest request(BucketName);
    auto outcome = client.GetBucketWebsite(request);

    if (outcome.isSuccess()) {
        std::cout << "GetBucketWebsite success,IndexDocument: " << outcome.result(). IndexDocument() <<
        " ,ErrorDocument: " << outcome.result(). ErrorDocument() << std::endl;
    }
    else { 
        /* Handle exceptions. */
        std::cout << "GetBucketWebsite fail" <<
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

## Delete static website hosting configurations {#section_o4l_h3l_ngb .section}

Use the following code to delete static website hosting configurations:

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
 
 
    /* Delete static website hosting configurations. */
    DeleteBucketWebsiteRequest request(BucketName);

    auto outcome = client.DeleteBucketWebsite(request);
  
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "DeleteBucketWebsite fail" <<
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

