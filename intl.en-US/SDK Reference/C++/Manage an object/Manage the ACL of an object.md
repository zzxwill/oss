# Manage the ACL of an object {#concept_90509_zh .concept}

This topic describes how to manage the access control list \(ACL\) of an object.

The following table describes the permissions included in the ACL of an object.

|Permission|Description|Value|
|:---------|:----------|:----|
|Inherit Bucket|The ACL of an object is the same as that of its bucket.|CannedAccessControlList::Default|
|Private|Only the owner or authorized users have read and write permissions on the object.|CannedAccessControlList::Private|
|Public Read|Only the owner or authorized users of an object have read and write permissions on the object. Other users \(including anonymous users\) have only read permissions on the object. Use caution when using this operation.|CannedAccessControlList::PublicRead|
|Public|All users of an object have read and write permissions on the object. Use caution when using this operation.|CannedAccessControlList::PublicReadWrite|

The ACL privileges of objects take precedence over those of buckets. For example, the ACL of a bucket is Private, while the object ACL is Public. \(All users of an object have read and write permissions on the object.\) If an object does not have an ACL, the ACL of its bucket is used by default.

## Configure an ACL of an object {#section_cvy_51z_kfb .section}

Use the following code to configure an ACL for a specified object:

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

    /* Set the ACL of the object. */
    SetObjectAclRequest request(BucketName, ObjectName);
    request.setAcl(CannedAccessControlList::Private);
    auto outcome = client.SetObjectAcl(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "SetObjectAcl fail" <<
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

## Obtain the ACL of an object {#section_rkt_5pl_ngb .section}

Use the following code to obtain an ACL to access a specified object:

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

    /* Obtain the ACL of the object. */
    GetObjectAclRequest request(BucketName, ObjectName);
    auto outcome = client.GetObjectAcl(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "GetObjectAcl fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    else { 
        std::cout << " GetObjectAcl success, Acl:" << outcome.result(). Acl() << std::endl;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

