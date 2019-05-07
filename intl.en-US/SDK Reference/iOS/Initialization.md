# Initialization {#concept_32057_zh .concept}

The OSSClient is the iOS client of OSS. It provides the caller with a series of methods to operate and manage buckets and objects. Before you use the OSS iOS SDK to initiate a request to OSS, you need to first initialize an OSSClient instance and complete necessary settings.

**Note:** 

Make sure that the lifecycle of the OSSClient is consistent with that of the application. When an application is started, create an OSSClient instance. When the application is ended, release the instance.

## Determine an endpoint {#section_mg4_xhj_lfb .section}

An endpoint is the URL of Alibaba Cloud OSS in a region. It supports the following two formats:

|Endpoint type|Description|
|:------------|:----------|
|OSS endpoint|The URL of the region where the bucket resides. For more information about the endpoints of each region, see [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).|
|Custom domain|The domain defined by the user. CNAME allows you to customize the OSS domain.|

-   OSS endpoints

    When you use the endpoint of a bucket, you can query the endpoint in either of the following ways:

    -   Query the relationship between the endpoint and region, as shown in [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).
    -   You can log on to the [OSS console](https://oss.console.aliyun.com/index#/). In the left-side navigation pane, locate the Buckets section, enter bucket-1 in the search box and click the search icon. Click the bucket name to go to the Overview tab. In the Domain Names section, the suffix of `bucket-1.oss-cn-hangzhou.aliyuncs.com` is `oss-cn-hangzhou.aliyuncs.com`, which is the endpoint for Internet access.
-   Custom domains

    Use CNAME to bind a custom domain to a bucket, and then use the domain to access objects in the bucket.

    For example, to bind the new-image.xxxxx.com domain to the bucket named image in China \(Shenzhen\), follow these steps: Contact your hosting provider for xxxxx.com to configure a new domain name resolution record. This record is used to resolve `http://new-image.xxxxx.com` to `http://image.oss-cn-shenzhen.aliyuncs.com`. The record type is CNAME.


## Configure an endpoint and credential { .section}

A mobile device is an untrusted environment. It is highly risky to store `AccessKeyId` and `AccessKeySecret` in a mobile device to sign requests. We recommend that you use STS Authentication Mode or Self-signed Mode. For more information, see [Authorized access](reseller.en-US/SDK Reference/iOS/Authorized access.md#) and [Direct transmission through a mobile device](../../../../reseller.en-US/Best Practices/Application server/Set up direct data transfer for mobile apps.md#).

**Note:** If the STS authentication mode is used, we recommend that you use OSSAuthCredentialsProvider to directly access STS. The token is automatically updated after it expires.

The following example shows you how to configure the Endpoint and CredentialProvider fields:

```language-objc
NSString *endpoint = "http://oss-cn-hangzhou.aliyuncs.com";

// Use the AccessKey ID and AccessKey Secret that are issued by Alibaba Cloud to construct a credential provider.
// We recommend that you use the plaintext AccessKey pair only for tests. For more information about authentication modes, see the "Authorized access" section.
id<OSSCredentialProvider> credential = [[OSSPlainTextAKSKPairCredentialProvider alloc] initWithPlainTextAccessKey:@"<your accessKeyId>" secretKey:@"<your accessKeySecret>"];

client = [[OSSClient alloc] initWithEndpoint:endpoint credentialProvider:credential];

```

## Set the Endpoint field to CNAME { .section}

If you bind CNAME to a bucket, you can set the Endpoint field to CNAME.

```language-objc
NSString *endpoint = "http://new-image.xxxxx.com";

id<OSSCredentialProvider> credential = [[OSSPlainTextAKSKPairCredentialProvider alloc] initWithPlainTextAccessKey:@"<your accessKeyId>" secretKey:@"<your accessKeySecret>"];

client = [[OSSClient alloc] initWithEndpoint:endpoint credentialProvider:credential];

```

**Note:** After Apple implemented the ATS standard, all `Endpoint` URLs must be HTTPS URLs, while domains set by `CNAME` do not support certificate settings. Therefore, you cannot use `CNAME` to set the `Endpoint` field.

## Enable access logging { .section}

Mobile devices are used in various environments. The OSS SDK cannot provide normal services in some regions or in a certain period of time. To further locate problems developers encounter, the OSS SDK records the log information locally after access logging is enabled. Before you use the OSSClient, initialize the OSSClient instance and call the following method to enable access logging:

```language-objc
//The log entry style.
//2017/10/25 11:05:43:863  [Debug]: the 17th: <NSThread: 0x7f8099108580>{number = 3, name = (null)}
//2017/10/25 11:05:43:863  [Debug]: the 15th:<NSThread: 0x7f80976052c0>
//2017/10/25 11:05:43:863  [Debug]: ----------TestDebug------------
[OSSLog enableLog];//Run this method to enable access logging.

```

**Note:** 

-   Logs are stored in the Caches/OSSLogs folder of the sandbox.
-   You can upload logs to the server to further track problems.

You can also access Alibaba Cloud Log Service to upload logs.

## Configure ClientConfiguration { .section}

Use the following code to configure ClientConfiguration in detail during the initialization process:

```language-objc
NSString *endpoint = "https://oss-cn-hangzhou.aliyuncs.com";

// We recommend that you use the plaintext AccessKey pair only for tests. For more information about authentication modes, see the "Authorized access" section.
id<OSSCredentialProvider> credential = [[OSSPlainTextAKSKPairCredentialProvider alloc] initWithPlainTextAccessKey:@"<your accessKeyId>" secretKey:@"<your accessKeySecret>"];
                                                                                                        
OSSClientConfiguration * conf = [OSSClientConfiguration new];
conf.maxRetryCount = 3; // The number of retry attempts after the network request encounters an exception and fails.
conf.timeoutIntervalForRequest = 30; // The timeout period of a network request.
conf.timeoutIntervalForResource = 24 * 60 * 60; // The maximum timeout period for resource transmission.

client = [[OSSClient alloc] initWithEndpoint:endpoint credentialProvider:credential clientConfiguration:conf];

```

## OSSTask {#section_ozn_44j_lfb .section}

An OSSTask is immediately returned for all operations that call APIs. Example:

```
OSSTask * task = [client getObject:get];
```

To implement an asynchronous callback, you can configure a continuation for the task. Example:

```
[task continueWithBlock: ^(OSSTask *task) {
    // do something
    ...
    return nil;
}];
```

You can also wait until the task is complete \(synchronous wait\). Example:

```
[task waitUntilFinished];
```

