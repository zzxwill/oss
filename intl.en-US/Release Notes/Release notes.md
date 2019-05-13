# Release notes {#concept_185831 .concept}

This topic records the released functions of OSS and lists the documentation related to the functions.

|Function|Description|Release date|Applied region|Documentation|
|--------|-----------|------------|--------------|-------------|
|ossutil 1.6.0| Ossutil allows you to manage OSS data easily by using command lines and provides simple and easy-to-use commands for bucket and object management. Ossutil supports the following operating systems: Windows, Linux, and Mac.

 In ossutil 1.6.0, the following functions are added: create folders, append upload, Cross-Origin Resource Sharing \(CORS\), manage logs, and set anti-leech.

 |2019-04-25|All regions| -   [Quick start](../../../../reseller.en-US/Tools/ossutil/Quick start.md#)
-   [Bucket-related commands](../../../../reseller.en-US/Tools/ossutil/Bucket-related commands.md#)
-   [Object-related commands](../../../../reseller.en-US/Tools/ossutil/Object-related commands.md#)

 |
|Bucket policy|Bucket policies are used for resource-based authorization and can be directly configured by the bucket owner on the graphical console for access authorization.|2019-01-21|All regions|[Use bucket policies to authorize other users to access OSS resources](../../../../reseller.en-US/Console User Guide/Upload、download and manage objects/Use bucket policies to authorize other users to access OSS resources.md#)|
|Real-time log query|By integrating OSS and Log Service \(SLS\), the real-time log query function allows you to query OSS access logs in the OSS console so that you can monitor OSS operations, measure OSS access statistics, trace exceptions, and locate problems with ease.|2018-12-26|Regions in Mainland China|[Real-time log query](../../../../reseller.en-US/Developer Guide/Manage logs/Real-time log query.md#)|
|Set or modify the storage class of an object|When uploading an object, you can set the storage class of the object to Standard, IA, or Archive. The setting takes effect immediately after the object is uploaded. You can convert the storage class of an object among Standard, IA, and Archive by performing a CopyObject operation. The time period that is required for the conversion to take effect reduces from days to seconds.

 |2018-11-10|All regions| -   [Overview](../../../../reseller.en-US/Developer Guide/Storage classes/Overview.md#)
-   [Convert between storage classes](../../../../reseller.en-US/Developer Guide/Storage classes/Convert between storage classes.md#)
-   [CopyObject](../../../../reseller.en-US/API Reference/Object operations/CopyObject.md#)

 |
|Terraform|The Terraform module for Alibaba Cloud OSS is released. By using Terraform, you can manage your infrastructure resources based on versions and perform OSS operations, such as creating a bucket or managing objects, by running code.|2018-11-07|All regions| -   [Introduction](../../../../reseller.en-US/Best Practices/Terraform/Introduction.md#)
-   [Use Terraform to manage OSS](../../../../reseller.en-US/Best Practices/Terraform/Use Terraform to manage OSS.md#)

 |
|Server-side encryption based on CMKs|You can encrypt objects at the server side by using KMS.|2018-10-20|All regions|[Protect data by performing server-side encryption](../../../../reseller.en-US/Best Practices/Data security/Protect data by performing server-side encryption.md#)|
|OSS Select|The OSS Select function allows you to use SQL statements to select required data from an object, reducing the data that needs to be transferred and improving the efficiency that you obtains data from OSS.|2018-09-28|All regions| -   [SelectObject](../../../../reseller.en-US/API Reference/Object operations/SelectObject.md#)

 |
|Compliant retention strategy|OSS supports compliant retention strategies for buckets, which protect yoor data from being deleted or modified.|2018-09-28|China South 1 \(Shenzhen\)| -   [Set a retention policy](../../../../reseller.en-US/Developer Guide/Compliant retention strategy/Introduction.md#)
-   [Set a compliant retention strategy](../../../../reseller.en-US/Developer Guide/Compliant retention strategy/Set a compliant retention strategy.md#)

 |
|Redundant storage across zones|Data in OSS is separately stored in three zones within the same region. Users can access the data even if one of the three zones is unavailable.|2018-09-28|China South 1 \(Shenzhen\) and China North 2 \(Beijing\)|[Redundant storage across zones](../../../../reseller.en-US/Developer Guide/Disaster recovery/Redundant storage across zones.md#)|
|Pay-by-requester mode|After the pay-by-requester mode is enabled for a bucket in OSS, the requester instead of the bucket owner pays the cost of the request and traffic. The bucket owner always pays the cost of storing data.|2018-09-27|China South 1 \(Shenzhen\)|[Enable the pay-by-requester mode](../../../../reseller.en-US/Developer Guide/Buckets/Enable the pay-by-requester mode.md#)|
|Use CMKs managed by KMS to decrypt data \(SSE-KMS\)|You can encrypt OSS data on the server side by using CMKs managed by KMS. You can specify the key for one CMK ID to achieve the BYOK function.|2018-08-14|All regions|[Server-side encryption](../../../../reseller.en-US/Developer Guide/Data encryption/Server-side encryption.md#)|
|OSS Python SDK supports client-side encryption|You can use the client-side encryption SDK to encrypt local data and upload the data to OSS. To use this function, you must manage the encryption process and the encryption key.|2018-06-05|All regions| -   [Client-side encryption](../../../../reseller.en-US/SDK Reference/Python/Client-side encryption.md#)

 |
|The unit price for the Standard storage class is reduced|The unit price for the Standard storage class in Mainland China reduces to US$0.0173/GB/Month, which is a 18.9% drop.|2018-06-02|Regions in Mainland China| -   [OSS pricing page](https://www.alibabacloud.com/product/oss/pricing?spm=a2c63.p38356.879954.7.3e2e3a54CJZuMt)

 |
|Combination of OSS and DataLakeAnalytics|You can query and analyze OSS data in the DataLakeAnalytics console in a serverless and interactive method.|2018-05-31|All regions| -   [Quickly analyze data in OSS](https://partners-intl.aliyun.com/help/doc-detail/70387.htm)

 |
|OSS Browser.js and Node.js SDKs support resumable upload|In resumable uploads, objects to be uploaded are divided into parts. All parts are combined into the complete object after they are uploaded.|2018-03-07|All regions| -   Node.js SDK: [Resumable upload](../../../../reseller.en-US/.md#)
-   Browser.js SDK: [Resumable upload](../../../../reseller.en-US/SDK Reference/Browser.js/Upload objects.md#section_snl_fxt_4fb)

 |
|Certificate hosting|You can host the certificate for your CNAME on OSS and access the CNAME through the HTTPS protocol.|2018-03-05|All regions| -   [Attach a custom domain name](../../../../reseller.en-US/Console User Guide/Manage buckets/Manage a domain/Attach a custom domain name.md#)
-   [Certificate hosting](../../../../reseller.en-US/Console User Guide/Manage buckets/Manage a domain/Certificate hosting.md#)

 |
|OSS iOS SDK can be called by SWIFT|Mobile application developers using SWIFT can call OSS iOS SDK.|2018-01-18|All regions|[OSSSwiftDemo](https://github.com/aliyun/aliyun-oss-ios-sdk/tree/master/OSSSwiftDemo)|
|OSS iOS SDK 2.8 and Android SDK 2.5 support CRC64 data verification|After the CRC64 verification is enabled, the CRC64 value of the uploaded or downloaded data is compared with the CRC64 value of the original data to ensure the data integrity.|2017-12-21|All regions| -   iOS SDK: [Data security](../../../../reseller.en-US/SDK Reference/iOS/Data security.md#)

 |
|Data migration tool ossimport|Ossimport is a tool used to migrate data from other cloud storage to OSS buckets.|2017-10-23|All regions| -   [Standalone deployment](../../../../reseller.en-US/Tools/ossimport/Standalone deployment.md#)
-   [Distributed deployment](../../../../reseller.en-US/Tools/ossimport/Distributed deployment.md#)

 |
|Cross-region replication|Cross-region replication is used to automatically and asynchronously copy objects across buckets in different regions. Any changes \(creation, replacement, and deletion\) to objects in the source bucket will be synchronized to the target bucket.|2017-09-15|Regions in Mainland China, US East 1, and US West 1|[Configure cross-region replication](../../../../reseller.en-US/Console User Guide/Manage buckets/Configure cross-region replication.md#)|
|The unit price for the Archive storage class is reduced|The unit price for the Archive storage class reduces by 45%. The minimum storage period is modified to 60 days.|2017-07-21|All regions| -   [Billing items](../../../../reseller.en-US/Pricing/Billing items.md#)
-   [RestoreObject](../../../../reseller.en-US/API Reference/Object operations/RestoreObject.md#)

 |
|The new version of the OSS console is released| -   Optimized the layout and navigation system.
-   Information in the overview page is aggregated.
-   The configuration and management functions of buckets and objects are upgraded.

 |2017-07-01|All regions|[Log on to OSS console](../../../../reseller.en-US/Console User Guide/Log on to the OSS console/Log on to OSS console.md#)|
|The maximum number of buckets that an Alibaba Cloud account can create increases to 30|An Alibaba Cloud account can create a maximum of 30 buckets in a region.|2017-04-24|All regions| -   [Limits](../../../../reseller.en-US/Product Introduction/Limits.md#)
-   [PutBucket](../../../../reseller.en-US/API Reference/Bucket operations/PutBucket.md#)

 |
|OSS iOS SDK 2.6.0 is released|OSS iOS SDK 2.6.0 supports the latest HTTPS access standards for App Store|2016-12-16|All regions|[Installation](../../../../reseller.en-US/SDK Reference/iOS/Installation.md#)|
|Manage parts|You can clear unnecessary parts in a timely manner by setting lifecycle rules.|2016-03-10|All regions|[Manage parts](../../../../reseller.en-US/Console User Guide/Manage fragments.md#)|
|OSS Media-C SDK is released|OSS Media-C SDK is released, which can be used to encapsulate data on camera equipment and upload the data to OSS.|2016-03-06|All regions|[Preface](../../../../reseller.en-US/SDK Reference/Media-C/Preface.md#)|
|Set back-to-origin rules|After you set back-to-origin rules, OSS retrieves requested data from the origin in multiple ways to meet your requirements such as hot data migration and specific request redirection.|2016-01-14|All regions|[Manage back-to-origin configurations](../../../../reseller.en-US/Developer Guide/Manage files/Manage back-to-origin configurations.md#)|
|OSS Ruby SDK is released|OSS Ruby SDK is released.|2015-11-26|All regions|[Installation](../../../../reseller.en-US/SDK Reference/Ruby/Installation.md#)|
|Image processing|The image processing function is enabled for buckets by default.|2015-11-10|All regions|[Image processing](../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#)|
|Append upload|By using append upload, you can use the AppendObject API of OSS to directly append content to the appendable objects that are uploaded.|2015-07-18|All regions|[Append upload](../../../../reseller.en-US/Developer Guide/Upload files/Append upload.md#)|
|Set data callback for application servers in upload tasks|OSS allows you to set up OSS-based direct data transfer for mobile applications and set data callback in upload tasks.|2015-07-08|All regions|[Set up data callback for mobile apps](../../../../reseller.en-US/Best Practices/Application server/Set up data callback for mobile apps.md#)|
|OSS supports RAM|OSS supports Resource Access Management \(RAM\). You can authorize a RAM user by using your Alibaba Cloud account or generate a temporary token for other users to access your OSS resources.|2015-04-26|All regions|[What is RAM and STS](../../../../reseller.en-US/Developer Guide/Hide/Access control/What is RAM and STS.md#)|
|Lifecycle rules|You can configure lifecycle rules for OSS buckets or objects by using the PutBucketLifecycle interface to automatically delete expired objects and parts or change the storage class of expired objects to IA or Archive, saving storage costs.|2014-10-20|All regions|[Manage object lifecycle](../../../../reseller.en-US//Manage lifecycle rules.md#)|
|Cross-Origin Resource Sharing \(CORS\)|CORS is a standard cross-origin solution provided by HTML5. OSS supports CORS for cross-region access.|2014-03-15|All regions|[Cross-origin resource sharing \(CORS\)](../../../../reseller.en-US/Best Practices/Bucket management/Cross-origin resource sharing (CORS).md#)|
|Form upload|By using form upload, you can use the PostObject API of OSS to upload objects with a maximum size of 5 GB.|2014-02-12|All regions|[Form upload](../../../../reseller.en-US/Developer Guide/Upload files/Form upload.md#)|
|Server-side encryption|OSS supports server-side encryption for uploaded data. This means that when user data is uploaded, OSS encrypts the data and permanently stores the data. Then, when the data is downloaded by a user, OSS automatically decrypts the data, returns the original data to the user, and declares in the header of the returned HTTP request that the data has been encrypted on the server.|2012-11-4|All regions|[Server-side encryption](../../../../reseller.en-US/Developer Guide/Data encryption/Server-side encryption.md#)|
|CNAME|To access an uploaded object by using a custom domain name, you must attach the custom domain name to the bucket where the object is stored and add a CNAME record that directs to the Internet domain name of the bucket.|2012-09-04|All regions|[Bind a custom domain](../../../../reseller.en-US/Developer Guide/Buckets/Bind a custom domain.md#)|
|Access logging|A large number of access logs are generated when an object is accessed. After the access logging function is enabled for a bucket, OSS automatically generates an object in the specified bucket \(target bucket\) to store the access logs on an hourly basis.|2012-08-09|All regions|[Access logging](../../../../reseller.en-US/Developer Guide/Manage logs/访问日志存储.md#)|
|Static website hosting|You can call the PutBucketWebsite API to set your bucket to the static website hosting mode and access the static website through the URL of the bucket.|2012-06-20|All regions|[Configure static website hosting](../../../../reseller.en-US/Developer Guide/Static website hosting/Configure static website hosting.md#)|
|Multipart upload|By using multipart upload and resumable upload provided by Alibaba Cloud OSS, you can split an object into multiple data blocks \(parts\) and upload them separately. After uploading all the object parts, you can call an API to combine them into an object.|2012-03-29|All regions|[Multipart upload and resumable upload](../../../../reseller.en-US/Developer Guide/Upload files/Multipart upload and resumable upload.md#)|
|Copy objects|You can copy objects from a bucket to another bucket without modifying the object content.|2011-12-16|All regions|[Copy objects](../../../../reseller.en-US/Developer Guide/Manage files/Copy objects.md#)|
|Anti-leech|By using the anti-leech function, you can set referers by calling the PutBucketReferer API to prevent your OSS data from being used by unauthorized users.|2011-12-16|All regions|[Configure hotlinking protection](../../../../reseller.en-US/Developer Guide/Buckets/Configure hotlinking protection.md#)|
|HTTP header|HTTP header is used to define the policy of HTTP requests, such as cache policy or download policy.|2011-12-16|All regions|[Set an HTTP header](../../../../reseller.en-US/Console User Guide/Upload、download and manage objects/Set an HTTP header.md#)|
|OSS is released|Alibaba Cloud OSS is released for commercial use.|2011-10-22|All regions|[What is OSS?](../../../../reseller.en-US/Product Introduction/What is OSS?.md#)|

