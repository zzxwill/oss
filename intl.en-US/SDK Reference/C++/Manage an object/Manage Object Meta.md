# Manage Object Meta {#concept_90504_zh .concept}

Object Meta contains HTTP headers and User Meta. For more information, see [Object Meta](../../../../../reseller.en-US/Developer Guide/Manage files/Object Meta.md#) in OSS Developer Guide.

## Configure Object Meta {#section_iwd_hql_ngb .section}

Use the following code to configure Object Meta:

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
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret , conf);
  
 
    /* Set the HTTP header. */
    auto meta = ObjectMetaData();
    meta.setContentType("text/plain");
    meta.setCacheControl("max-ag e=3");
    /* Set User Meta. */
    meta.UserMetaData()["meta"] = "meta-value";

    std::shared_ptr<std::iostream> content = std::make_shared<std::stringstream>(); 
    *content << "Thank you for using Aliyun Object Storage Service!" ;
    client.PutObject(BucketName, ObjectName, content, meta);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "PutObject fail" <<
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

## Obtain Object Meta {#section_l1n_dbz_kfb .section}

The following table lists the methods you can use to obtain Object Meta.

|Method|Description|Benefit|
|:-----|:----------|:------|
|GetObjectMeta|Obtains some Object Meta such as the ETag, Size, and LastModified \(the latest time an object was modified\) values of the object.|More lightweight and faster|
|HeadObject|Obtains all Object Meta.|None|

Use the following code to obtain Object Meta:

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

    /* Obtain some Object Meta. */
    auto outcome = client.GetObjectMeta(BucketName, ObjectName);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "GetObjectMeta fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    else { 
        auto metadata = outcome.result();
        std::cout << " get metadata success, ETag:" << metadata.ETag() << "; LastModified:" 
        << metadata.LastModified() << "; Size:" << metadata.ContentLength() << std::endl;
    }
    
    /* Obtain all Object Meta. */
    outcome = client.HeadObject(BucketName, ObjectName);
 
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "HeadObject fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    else { 
        auto headMeta = outcome.result();
        std::cout <<"headMeta success, ContentType:" 
        << headMeta.ContentType() << "; ContentLength:" << headMeta.ContentLength() 
        << "; CacheControl:" << headMeta.CacheControl() << std::endl;
    }
  
    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

**Note:** For more information about HTTP headers, see [RFC2616](https://tools.ietf.org/html/rfc2616).

