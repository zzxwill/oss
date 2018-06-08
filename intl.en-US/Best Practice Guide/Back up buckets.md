# Back up buckets {#concept_kzh_w4f_vdb .concept}

Alibaba Cloud offers multiple backup methods for data on OSS to suit different scenarios.

The following methods can be used to back up OSS data on the cloud:

-   Cross-region replication \(set on the console or using APIs or SDK code\)
-   OssImport tool

## Back up data using cross-region replication {#section_m2p_y4f_vdb .section}

Applicable scenarios

See [Cross-Region replication development guide](../intl.en-US/Developer Guide/Managing Objects/Cross-region replication.md#).

Operation on the console

See [Cross-Region replication operation guide](../intl.en-US/Console User Guide/Manage buckets/Set cross-region replication.md#).

FAQs

See [FAQs about data synchronization between buckets](https://www.alibabacloud.com/help/faq-detail/62987.htm).

NOTE

-   The source bucket and target bucket belong to the same user but different regions.
-   The source bucket and target bucket do not use archive storage.
-   Data synchronization between buckets in the same region can be implemented using OSS SDK/API code.

## Back up data using the OssImport tool {#section_ftg_dpf_vdb .section}

The OssImport tool can migrate data stored on local hosts or other cloud storage systems to OSS. It has the following features:

-   Supports a vast variety of data sources, including local drives, Qiniu Cloud, Baidu BOS, AWS S3, Azure Blob,  Blob, but also cloud, Tencent cloud cos, Golden Mountain ks3, HTTP, OSS, and so on, and can be expanded as needed.
-   Supports resumable upload.
-   Supports throttling.
-   Supports migration of objects generated after a specified time or with a specified prefix.
-   Supports parallel data upload and download.
-   Supports the standalone and distributed modes.  The standalone mode is easy to deploy and use, while the distributed mode is suitable for large-scale data migration.

Applicable scenarios

See [Data migration](../intl.en-US/Utilities/ossimport/Data migration.md#).

Installation and deployment

See [Architecture and configuration](../intl.en-US/Utilities/ossimport/Architecture and configuration.md#), [Standalone deployment](../intl.en-US/Utilities/ossimport/Standalone deployment.md#),[Distributed deployment](../intl.en-US/Utilities/ossimport/Distributed deployment.md#).

FAQ

See[FAQs](../intl.en-US/Utilities/ossimport/FAQs.md#).

NOTE

-   If data needs to be migrated between buckets of different user accounts and the data volume exceeds 10 TB, the distributed version is recommended.
-   When using the incremental mode to synchronize object changes between OSS buckets, note that OssImport can synchronize only modification operations \(put/append/multipart\) and cannot synchronize read or delete operations.  No SLA guarantee is provided for timely data synchronization in this mode. Therefore, use the incremental mode with caution.
-   Cross-region replication is recommended for data synchronization between different regions, if cross-region replication is enabled in these regions.

