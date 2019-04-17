# Lifecyle rules {#concept_32140_zh .concept}

This topic describes how to manage lifecycle rules.

You can configure lifecycle rules for OSS buckets or objects to automatically delete expired objects and parts or change the storage class of expired objects to IA or Archive, saving storage costs. A lifecycle rule includes the following elements:

-   Rule ID: Identifies a lifecycle rule. A rule ID is unique in a bucket.
-   Strategy: You can set one of the following two application strategies for a rule. The strategy for all rules set for a bucket must be the same.
    -   The rule applies to objects with specified prefixes. You can create multiple rules with this strategy. Prefixes specified in different rules must be different.
    -   The rule applies to the entire bucket. You can create only one rule with this strategy.
-   Expiration period:
    -   Specifies the expiration period \(N days\) of a lifecycle rule. An object expires N days after it is modified for the last time.
    -   Specifies an expiration time. Objects modified before the time expire.
-   Status: Indicates whether the rule is enabled or disabled.

Lifecycle rules apply to resources uploaded by uploadPart. The last modification time of an object is the time the multipart upload event was initiated.

For more information about lifecycle management, see [Manage object lifecycle](../../../../reseller.en-US/Developer Guide/Manage files/Manage lifecycle rules.md#).

## Configure lifecycle rules {#section_v5d_yhz_kfb .section}

Use the following code to configure lifecycle rules:

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
  
    SetBucketLifecycleRequest request(BucketName);
    std::string date("2022-10-12T00:00:00. 000Z") ;
   
    /* Specify the period to retain objects after their most recent modification time. After the period expires, the objects are deleted. */
    auto rule1 = LifecycleRule();
    rule1.setID("rule1");
    rule1.setPrefix("test1/");
    rule1.setStatus(RuleStatus::Enabled);
    rule1.setExpiration(3);
  
    /* Specify the date before which objects are created. After the date, the objects expire. */
    auto rule2 = LifecycleRule();
    rule2.setID("rule2");
    rule2.setPrefix("test2/");
    rule2.setStatus(RuleStatus::Disabled);
    rule2.setExpiration(date);

    /* Configure lifecycle rules. */
    LifecycleRuleList list{rule1, rule2};
    request.setLifecycleRules(list);
    auto outcome = client.SetBucketLifecycle(request);
  
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "SetBucketLifecycle fail" <<
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

## View lifecycle rules {#section_vkw_24l_ngb .section}

Use the following code to view lifecycle rules:

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
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, c onf);
   
    /* View lifecycle rules. */
    auto outcome = client.GetBucketLifecycle(BucketName);
  
    if (outcome.isSuccess()) {
        std::cout << "GetBucketLifecycle success," << std::endl;
        for (auto const rule : outcome.result(). LifecycleRules()) {
            std::cout << "rule:" << rule.ID() << "," << rule.Prefix() << "," << rule.Status() << ","
            "hasExpiration:" << rule.hasExpiration() << "," <<
            "hasTransitionList:" << rule.hasTransitionList() << "," << std::endl;
        }
    }
    else {
        /* Handle exceptions. */
        std::cout << "GetBucketLifecycle fail" <<
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

## Clear lifecycle rules {#section_wkw_24l_ngb .section}

Use the following code to clear lifecycle rules:

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
  
    /* Clear lifecycle rules. */
    DeleteBucketLifecycleRequest request(BucketName);
    auto outcome = client.DeleteBucketLifecycle(request);
  
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "DeleteBucketLifecycle fail" <<
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

