# What is OSS? {#concept_ybr_fg1_tdb .concept}

Alibaba Cloud Object Storage Service \(OSS\) is a storage service that enables you to store, back up, and archive any amount of data in the cloud. OSS is a cost-effective, highly secure, and highly reliable cloud storage solution. It uses RESTful APIs and is designed for 99.999999999% \(11 nines\) durability and 99.99% availability. Using OSS, you can store and retrieve any type of data at any time, from anywhere on the web.

You can use API and SDK interfaces provided by Alibaba Cloud or OSS migration tools to transfer massive amounts of data into or out of Alibaba Cloud OSS. You can use the Standard storage class of OSS to store image, audio, and video files for apps and large websites. You can use the Infrequent Access \(IA\) or Archive storage class as a low-cost solution for backup and archiving of infrequently accessed data.

## Concepts {#section_h4j_rlb_n2b .section}

-   Storage Class

    OSS provides three storage classes: Standard, Infrequent Access, and Archive. These storage classes cover various data storage scenarios from hot data to cold data. For more information, see [Introduction to storage classes](../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#).

-   Bucket

    A bucket is a container for objects stored in OSS. Every object is contained in a bucket. The data model structure of Alibaba Cloud OSS is flat instead of hierarchical.

-   Objects

    Objects, also known as files, are the fundamental entities stored in OSS. An object is composed of metadata, data, and key. The key is the unique object name in a bucket. Metadata defines the attributes of an object, such as the time last modified and the object size. You can also specify custom metadata of an object.

-   Region

    A region represents the physical location of an OSS data center. You can choose the region where OSS will store the buckets you create. You may choose a region that has the least latency, lowest costs, or that meets certain regulatory requirements. Generally, the closer the user is in proximity to a region, the faster the access speed is. For more information, see [OSS regions and endpoints](../../../../reseller.en-US/Developer Guide/Regions and endpoints.md#).

-   Endpoint

    An endpoint is the domain name used to access the OSS. OSS provides external services through HTTP RESTful APIs. Different regions use different endpoints. For the same region, access through an intranet or through the Internet also uses different endpoints. For more information, see [OSS regions and endpoints](../../../../reseller.en-US/Developer Guide/Regions and endpoints.md#).

-   AccessKey

    An AccessKey \(AK\) is composed of an AccessKeyId and an AccessKeySecret.  They work in pairs to perform access identity verification. OSS verifies the identity of a request sender by using the AccessKeyId/AccessKeySecret symmetric encryption method. The AccessKeyId is used to identify a user. The AccessKeySecret is used for the user to encrypt the signature and for OSS to verify the signature. The AccessKeySecret must be kept confidential.


## Use OSS {#section_o5k_1mb_n2b .section}

Alibaba Cloud provides an intuitive operation interface for you to manage your OSS resources. You can log on to the OSS console to operate your buckets and objects. For more information, see the *OSS Console User Guide*.

You can also use APIs and SDKs to manage your OSS resources. For more information, see [OSS API Reference](../../../../reseller.en-US/API Reference/API overview.md#) and [OSS SDK Reference](https://partners-intl.aliyun.com/help/doc-detail/52834.htm).

## OSS pricing {#section_sv5_nmb_n2b .section}

Traditional storage providers require you to purchase a predetermined amount of storage and network transfer capacity. If you exceed the capacity, your service is shut off or you are charged excess fees. If you do not use the full capacity, you still pay as though you have used it all.

OSS charges you only for what you actually use, without excess fees. As your business grows, you can enjoy the cost advantages of the flexible infrastructure from Alibaba Cloud, which adapts to meet your ever-changing requirements.

