# Benefits {#concept_nbc_rh1_tdb .concept}

Alibaba Cloud Object Storage Service \(OSS\) is a storage service that enables you to store, back up, and archive any amount of data in the cloud. OSS is a cost-effective, highly secure, and highly reliable cloud storage solution. This topic compares OSS with the traditional storage to help you better understand Alibaba Cloud OSS.

## Benefits of OSS over traditional storage {#section_9tw_4lp_yzs .section}

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

## More benefits of OSS {#section_io7_dk2_y19 .section}

-   Easy to use
    -   Provides RESTful APIs, a wide range of SDKs, client tools, and a web console. You can easily upload, download, retrieve, and manage massive amounts of data for websites and applications in the same way as for regular files in Windows.
    -   Sets no limit on the number and size of files. Unlike the traditional hardware storage, OSS enables you to easily scale up \(expand\) your storage space as needed.
    -   Supports streaming upload and download, which is suitable for business scenarios where you need to simultaneously read and write videos and other large files.
    -   Offers lifecycle management. You can delete expired data in batches or transition the data to low-cost archive services.
-   Powerful and flexible security
    -   Provides flexible authentication and authorization, including STS, URL, whitelist, anti-leeching, and RAM account features.
    -   Offers user-level resource isolation. You can also use the multi-cluster synchronization service.
-   Data redundancy mechanism

    OSS uses a data redundancy storage mechanism to store redundant data of each object on multiple devices of different facilities in the same area, ensuring data reliability and availability in case of hardware failure.

    -   Object operations in OSS are strongly consistent. For example, once a user receives an upload or copy success response, the object can be read immediately, and the redundant data has already been written to multiple devices.
    -   To ensure complete data transmission, OSS checks whether an error occurs when packets are transmitted between the client and the server by calculating the checksum of the network traffic packets.
    -   The redundant storage mechanism of OSS can avoid data loss if two storage facilities are damaged at the same time.
        -   After data is stored in OSS, OSS checks whether redundant data is lost. If yes, OSS recovers the lost redundant data to ensure data reliability and availability.
        -   OSS periodically checks the integrity of data through verification to discover data damage caused by factors such as hardware failure. If data is partially damaged or lost, OSS reconstructs and repairs the damaged data by using redundant data.
-   Rich and powerful value-added services
    -   Image processing: Supports format conversion, thumbnails, cropping, watermarks, scaling, and other operations with a wide variety of file formats including jpg, png, bmp, gif, webp, and tiff.
    -   Audio/video transcoding: Provides high-quality, high-speed, parallel audio/video transcoding capabilities for audio/video files stored in OSS. You can easily make your audio/video files compatible for different types of devices.
    -   Accelerated content delivery: Content Delivery Network \(CDN\) can be used with OSS to speed up the delivery of content stored in OSS. This service features high stability, unlimited origin bandwidth, and easy configuration.

