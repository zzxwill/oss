# FAQs on using old and new versions of APIs and domain names {#concept_tmj_dxc_wdb .concept}

## There are major differences between new and old versions of APIs: {#section_ir2_3xc_wdb .section}

-   New version API: `http://bucket.<endpoint>/object?x-oss-process=image/action,parame_value`

    All image manipulation operations are passed via `x-oss-process`. Each action is executed sequentially without any need for channel management.

-   Old versionAPI: `http://channel.<endpoint>/object@action.format`

    It can be processed as a separator by`@`.


## What are the advantages of OSS domain names when used with the Image Service? {#section_ot1_kxc_wdb .section}

|Item|Use IMG domain access|Direct use of OSS domain name access|
|----|---------------------|------------------------------------|
|Use|Store and process two Domain Name Systems|One-stop processing for upload, management, process, distribution.|
|Is new version of API supported?|Supported|Supported|
|Is old version of API supported?|Supported|Not supported by default|
|Is https supported?|Not supported.|Supported|
|Is VPC Network supported?|Not supported|Supported|
|Is multi-domain binding supported?|Not supported|Supported|
|Is source station update automatically refresh Alibaba CDN supported?|Not supported|Supported|

**Note:** 

-   When OSS domain names are being used, only APIs for the new version of the ING service can be used. When IMG domain names are being used, APIs for the old and new versions of the IMG service can be used.
-   If the IMG domain name is expected to be capable of multi-CDN acceleration, the IMG domain name can be directly accessed by configuring the CDN to go back to the source host, and domain name binding is not required to complete the CDN acceleration.

## What is the logic here for the two API methods and the two domain name access methods on the console? {#section_un4_mxc_wdb .section}

Bucket processed before enabling the old version of image 

-   To keep the logic consistent with the original, the user sees the Domain Name of the old version of IMG, and custom domain names that have previously been bound.
-   The user's original graph protection configuration on the IMG domain name has no effect on the OSS domain name. When you start the same step in cross-region replication, the original graph protection and style separator are synchronized to the OSS domain name.
-   When the user closes the image processing service for the current bucket, the style configuration and domain name binding are cleared, and automatically jump to the new page.

Newly created bucket or a bucket that has not previously opened the IMG service:

-   The default is to be able to use the image processing service, which does not need to be up or turned off.
-   No need to bind domain names, the domain name binding operation is directly consistent with the domain name management of the bucket itself.

## If I’m currently using APIs for the old version of the IMG service, how do I switch to OSS domain names? {#section_ms1_4xc_wdb .section}

Currently, APIs for the old version of the IMG service cannot be used with OSS domain names without a request being sent to Alibaba Cloud. To request use of APIs for the old version, submit a ticket to Alibaba Cloud asking for this service. For style-based access, both OSS and IMG domain names can be used. If all your images are accessed by style, follow these steps to switch to the use of OSS domain names:

1.  Enable configuration synchronization in the current Image Service configurations, so that style separators and the source image protection feature can be synchronized to OSS domain names.
2.  If you use a custom domain name, direct its CNAME to the OSS domain name.

## Are style configurations the same for IMG and OSS domain names? {#section_icb_pxc_wdb .section}

All style configurations are shared by IMG and OSS domain names. Style configurations for IMG domain names can be applied to OSS domains.

