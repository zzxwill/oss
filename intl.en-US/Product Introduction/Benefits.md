# Benefits {#concept_nbc_rh1_tdb .concept}

## Benefits of OSS over traditional storage {#section_g4k_ky2_tdb .section}

|Item|OSS|Traditional storage|
|:---|:--|-------------------|
|Reliability| -   Guarantees 99.99% designed service availability.
-   Offers automatic scaling without affecting external services.
-   Guarantees 99.999999999% \(11 nines\) designed durability.
-   Offers automatic redundant data backup.

 | -   Depends on hardware reliability. Traditional storage has a relatively high failure rate. If a disk has bad sectors, data may be lost and cannot be recovered.
-   Manual data recovery is complex and involves a lot of time and technical resources.

 |
|Security| -   Provides enterprise-grade, multilevel security.
-   Supports multi-user resource isolation and remote disaster recovery.
-   Provides authentication and authorization, as well as whitelist, anti-leeching, and RAM account features.

 | -   Traffic cleaning service and black hole service must be purchased separately.
-   Security must be implemented independently.

 |
|Cost| -   BGP backbone network without bandwidth restrictions. Upstream traffic is free of charge.
-   No maintenance staff or hosting fees required.

 | -   Storage space is limited by hardware capacity. Manual scaling is required. 
-   Access speeds are slow during single or double-line access. Bandwidth restrictions are imposed. Manual scaling is required during peak traffic periods.
-   Requires professional maintenance staff and high costs.

 |
|Data Processing Capabilities|Provides image processing, audio/video transcoding, accelerated content delivery, archive services, and other value-added data services.|Must be purchased and deployed separately.|

## More benefits of OSS {#section_lc4_21b_tdb .section}

-   Easy to use
    -   Provides RESTful APIs, a wide range of SDKs, client tools, and a web console. You can easily upload, download, retrieve, and manage massive amounts of data for websites and applications in the same way as for regular files in Windows.
    -   Sets no limit on the number and size of files. Unlike the traditional hardware storage, OSS enables you to easily scale up \(expand\) your storage space as needed.
    -   Supports streaming upload and download, which is suitable for business scenarios where you need to simultaneously read and write videos and other large files.
    -   Offers lifecycle management. You can delete expired data in batches or transition the data to low-cost archive services.
-   Powerful and flexible security
    -    Provides flexible authentication and authorization, including STS, URL, whitelist, anti-leeching, and RAM account features.
    -   Offers user-level resource isolation. You can also use the multi-cluster synchronization service.
-   Rich and powerful value-added services
    -   Image processing: Supports format conversion, thumbnails, cropping, watermarks, scaling, and other operations with a wide variety of file formats including jpg, png, bmp, gif, webp, and tiff.
    -   Audio/video transcoding: Provides high-quality, high-speed, parallel audio/video transcoding capabilities for audio/video files stored in OSS. You can easily make your audio/video files compatible for different types of devices.
    -   Accelerated content delivery: Content Delivery Network \(CDN\) can be used with OSS to speed up the delivery of content stored in OSS. This service features high stability, unlimited origin bandwidth, and easy configuration.

