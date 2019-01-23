# Introduction to storage classes {#concept_fcn_3xt_tdb .concept}

OSS provides three storage classes: Standard, Infrequent Access, and Archive. These storage classes cover various data storage scenarios from hot data to cold data.

## Standard {#section_r3z_lxt_tdb .section}

OSS Standard storage provides highly reliable, highly available, and high-performance object storage service that supports frequent data access. The high throughput and low latency of OSS make it well suited for storing social networking content such as images, audio, and video. It is also great for storing large unstructured data sets for use in big data analytics.

OSS Standard storage has the following features:

-   Designed for 99.999999999% \(11 nines\) durability.
-   Designed for 99.99% availability.
-   High-throughput and low-latency access performance.
-   Supports HTTPS.
-   Supports Image Processing.

## Infrequent Access {#section_t3z_lxt_tdb .section}

OSS Infrequent Access storage is suitable for storing long-lived, but less-frequently accessed data \(once or twice per month\). With a storage unit price lower than the Standard class, it is suitable for longer-term backup of various mobile apps, smart device data, and enterprise data. Objects of the Infrequent Access storage class have a minimum storage duration. Charges apply if you delete objects that have been stored for less than 30 days. Objects of the Infrequent Access storage class have a minimum billable size. Objects smaller than 64 KB are charged as 64 KB. Data retrieval incurs charges.

OSS Infrequent Access storage has the following features:

-   Designed for 99.999999999% \(11 nines\) durability.
-   Designed for 99.99% service availability.
-   Supports real-time access.
-   Supports HTTPS.
-   Supports Image Processing.
-   Specified minimum storage duration and minimum billable size.

## Archive {#section_v3z_lxt_tdb .section}

OSS Archive storage has the lowest price among the three storage classes. It is suitable for storing archival data for a long time \(more than half a year recommended\), such as medical images, scientific materials, and video footages. The data is infrequently accessed during the storage period and it may take about one minute to restore the data to a readable state. Objects of the Archive storage class have a minimum storage duration. Charges apply if you delete objects that are stored for less than 60 days. Objects of the Archive storage class have a minimum billable size. Objects smaller than 64 KB are charged as 64 KB. Data retrieval incurs charges.

OSS Archive storage has the following features:

-   Designed for 99.999999999% \(11 nines\) durability.
-   Designed for 99.99% service availability \(restored data\).
-   It takes one minute to restore the stored data from the frozen state to the readable state.
-   Supports HTTPS.
-   Supports Image Processing, but data needs to be restored first.
-   Specified minimum storage duration and minimum billable size.

## Comparison of storage classes { .section}

|Item|Standard|Infrequent Access|Archive|
|:---|:-------|:----------------|:------|
|Data durability|99.999999999%|99.999999999%|99.999999999%|
|Designed service availability|99.99%|99.99%|99.99% \(restored data\)|
|Minimum billed size of objects|Calculate by actual size of objects|64 KB|64 KB|
|Minimum storage duration|Not required|30 days|60 days|
|Data retrieval fee|No data retrieval fee|Charged by the size of retrieved data, in GB|Charged by the size of restored data, in GB|
|Latency|Latency in ms|Latency in ms|It takes one minute to restore data from the frozen state to the readable state.|
|Images processing|Supported|Supported|Supported, but data needs to be restored first.|

**Note:** 

 "Data" in "data retrieval fee" refers to the size of data read from the underlying distributed storage system. The data transferred over the public network is billed as part of the outbound traffic costs. 

## Supported APIs {#section_pfz_1yt_tdb .section}

|API|Standard|Infrequent Access|Archive|
|:--|:-------|:----------------|:------|
|Bucket creation, deletion, and query|
|PutBucket|Supported|Supported|Supported|
|GetBucket|Supported|Supported|Supported|
|DeleteBucket|Supported|Supported|Supported|
|Bucket ACL|
|PutBucketAcl|Supported|Supported|Supported|
|GetBucketAcl|Supported|Supported|Supported|
|Bucket logging|
|PutBucketLogging|Supported|Supported|Supported|
|GetBucketLogging|Supported|Supported|Supported|
|Bucket default static page|
|PutBucketWebsite|Supported|Supported|Not supported|
|GetBucketWebsite|Supported|Supported|Not supported|
|Bucket anti-leech protection|
|PutBucketReferer|Supported|Supported|Supported|
|GetBucketReferer|Supported|Supported|Supported|
|Bucket lifecycle|
|PutBucketLifecycle|Supported|Supported|Supported, data deletion only|
|GetBucketLifecycle|Supported|Supported|Supported|
|DeleteBucketLifecycle|Supported|Supported|Supported|
|Bucket Cross-Origin Replication| | | |
|PutBucketReplication|Supported|Supported|Supported|
|Bucket Cross-Origin Resource Sharing|
|PutBucketcors|Supported|Supported|Supported|
|GetBucketcors|Supported|Supported|Supported|
|DeleteBucketcors|Supported|Supported|Supported|
|Object operations|
|PutObject|Supported|Supported|Supported|
|PutObjectACL|Supported|Supported|Supported|
|GetObject|Supported|Supported|Supported, but data needs to be restored first|
|GetObjectACL|Supported|Supported|Supported|
|GetObjectMeta|Supported|Supported|Supported|
|HeadObject|Supported|Supported|Supported|
|CopyObject|Supported|Supported|Supported|
|OptionObject|Supported|Supported|Supported|
|DeleteObject|Supported|Supported|Supported|
|DeleteMultipleObjects|Supported|Supported|Supported|
|PostObject|Supported|Supported|Supported|
|PutSymlink|Supported|Supported|Supported|
|GetSymlink|Supported|Supported|Supported|
|RestoreObject|Not supported|Not supported|Supported|
|Multipart operations|
|InitiateMultipartUpload|Supported|Supported|Supported|
|UploadPart|Supported|Supported|Supported|
|UploadPartCopy|Supported|Supported|Supported|
|CompleteMultipartUpload|Supported|Supported|Supported|
|AbortMultipartUpload|Supported|Supported|Supported|
|ListMultipartUpload|Supported|Supported|Supported|
|ListParts|Supported|Supported|Supported|
|Image Processing|Supported|Supported|Supported|

