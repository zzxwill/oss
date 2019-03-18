# Overview {#concept_whm_jcq_tdb .concept}

The Object Storage Service \(OSS\) is a cloud storage service provided by Alibaba Cloud, featuring a massive capacity, security, a low cost, and high reliability. You can upload and download data anytime, anywhere, and on any Internet device through a simple RESTful interface described herein. With the OSS, you can develop a diverse range of massive data-based services such as multimedia sharing websites, online storage, personal data backups, and corporate data backups.

## Limits {#section_gyt_hkl_s2b .section}

Different OSS resources and functions have different limits. For more information, see [Limits](../../../../../intl.en-US/Product Introduction/Limits.md#).

## Usage {#section_vkl_wkl_s2b .section}

This topic describes the request syntax, request samples and return samples for each interface. If you want to perform additional development, we recommend you use OSS SDKs. For more information about the installation and usage of OSS SDKs, see [OSS SDK introduction](https://help.aliyun.com/document_detail/52834.html).

## Pricing {#section_khp_1pl_s2b .section}

For more information about the price of OSS, see [OSS pricing page](https://www.alibabacloud.com/product/oss#pricing).

## Terms {#section_xr1_spl_s2b .section}

|Term|Description|
|----|-----------|
|Bucket|A bucket is a resource in Alibaba Cloud that operates similar to a container and is used to store objects in OSS. Every object is contained in a bucket.|
|Object|An object \(sometimes referred to as a file\) is the fundamental storage resource in Alibaba Cloud OSS. An object is composed of metadata, data, and a key, in which the key is a unique name for the object.|
|Region|A region indicates the physical location of an Alibaba Cloud data center. You can choose the region in which the buckets you create are stored based on your costs and the geographic area from where requests to your resources are coming from. For more information, see [Regions and endpoints](../../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).|
|Endpoint|An endpoint is a domain name used to access OSS. OSS provides external services through HTTP RESTful APIs. You must use different endpoints to access different OSS regions, or access the same OSS region through the intranet and the Internet. For more information, see [Regions and endpoints](../../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).|
|AccessKey|An AccessKey \(AK\) is composed of an AccessKeyId and an AccessKeySecret, and is used to verify the identity of an entity that requests access to resources. OSS verifies the identity of a request sender by using symmetric encryption. The AccessKeyId is used to identify a user, and the AccessKeySecret is used by the user to encrypt the signature, and for OSS to verify the signature. The AccessKeySecret must be kept confidential.|

