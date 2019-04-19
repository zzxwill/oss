# Introduction {#concept_lnj_4rl_cfb .concept}

Alibaba Cloud OSS fully supports the Write Once, Read Many \(WORM\) feature and allows users to store and use data in a manner that they cannot delete or modify the data, which conforms to the compliance requirements of Securities and Exchange Commission \(SEC\) and Financial Industry Regulation Authority \(FINRA\) of USA.

**Note:** 

-   As the only cloud service certificated by Cohasset Associates in China, Alibaba Cloud OSS is in compliance with the strict electronic archiving requirements of various regulations, such as EC Rule 17a-4\(f\), FINRA 4511, and CFTC 1.31. For more information, see [OSS Cohasset Assessment Report](http://gosspublic.alicdn.com/OSSCohassetAssessmentReport.pdf).
-   The compliant retention strategy feature is now in the beta testing phase in the China South 1 \(Shenzhen\) region and will apply to all regions in the future.
-   You can only set a compliant retention strategy for buckets in OSS.

OSS supports strong compliant retention strategies. You can set a time-based compliant retention strategy for your buckets. After the strategy is locked, all users can upload objects to the buckets or read objects in the bucket. However, before the retention period for the objects expires, the protected objects and the retention strategy cannot be deleted. You can delete an object protected by the retention strategy only when the retention period expires. The WORM feature supported by OSS applies to various industries, such as finance, insurance, medical, and securities. You can build a "cloud-based compliant storage space" based on OSS.

**Note:** During the valid period of a compliant retention strategy, you can [set lifecycle rules](reseller.en-US/Developer Guide/Manage files/Manage lifecycle rules.md#) to convert the storage class of the objects in the bucket protected by the strategy, which reduces storage costs while ensuring the compliance.

## Operation {#section_gq3_zz5_cfb .section}

You can set a compliant retention strategy in the OSS console. For more information, see [Set a compliant retention strategy](../../../../reseller.en-US/Developer Guide/Compliant retention strategy/Set a compliant retention strategy.md#).

## Description {#section_nk2_fz5_cfb .section}

Currently, you can set only one time-based retention strategy with a retention period ranging from 1 day to 70 years.

Assume that you created a bucket named examplebucket on June 1, 2013, and then uploaded three objects named file1.txt, file2.txt, and file3.txt to the bucket at different times. After that, a compliant retention strategy was created for the bucket on July 1, 2014 with a protection period of 5 years. The following table describes the upload dates and expiration dates of the preceding three objects.

|Object|Upload date|Expiration date|
|:-----|:----------|:--------------|
|file1.txt|June 1, 2013|May 31, 2018|
|file2.txt|July 1, 2014|June 30, 2019|
|file3.txt|September 30, 2018|September 29, 2023|

-   Effective rules

    After a time-based retention strategy is created for a bucket, it is in the InProgress state by default, and the state is valid for 24 hours. Within the validity period, resources that apply to the strategy in the bucket are protected.

    -   Within 24 hours after a compliant retention strategy is created for a bucket, the bucket owner and authorized users can delete the strategy if it is not locked. If the compliant retention strategy is locked, it cannot be deleted and the protection period of the strategy cannot be shorten. However, you can extend the protection period of the strategy.
    -   If a compliant retention strategy is not locked after it is created for 24 hours, it is not locked.
    -   If you try to delete or modify the data in a bucket protected by a compliant retention strategy, OSS API returns the `409 FileImmutable` message.
-   Deletion rules
    -   A time-based compliant retention strategy is a metadata property of a bucket. When a bucket is deleted, the compliant retention strategy and access strategies set for the bucket are also deleted. Therefore, the owner of an empty bucket can delete the retention strategy set for the bucket by deleting the bucket.
    -   If a compliant retention strategy is not locked within 24 hours after it is created for a bucket, the bucket owner and authorized users can delete it.
    -   If a bucket stores objects which are in the protection period of a compliant retention strategy, you cannot delete neither the bucket nor the strategy set for the bucket.

