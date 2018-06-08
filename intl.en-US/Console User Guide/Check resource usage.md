# Check resource usage {#concept_t4l_sxm_vdb .concept}

## Overview {#section_mwm_txm_vdb .section}

You can check the usage of the following resources on the OSS console:

-   Basic data: Including bucket, data used, and requests per hour
-   Hotspot statistics: Including PV/UV, original, and hot spot
-   API statistics: Including method statistics and return code
-   Object access statistics: Including statistics about object access

This document uses the basic data as an example to describe the resource checking method.

## Procedures {#section_tgw_yxm_vdb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  In the bucket list on the left, click the target bucket name to open its information page.
3.  Click the Basic Data tab, and diagrams of the following three kinds of basic data are displayed, as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4766/2285_en-US.png)

    -   Bucket
    -   Data Used
    -   Requests per Hour
    The following three tables describe the basic data items included in the three diagrams and the description of the items:

    |**Basic Data**|**Description**|
    |Standard|Size of data stored in the standard type|
    |Archive|Size of data stored in the archive type|
    |Infrequent Access|Size of data stored in the infrequent access type|
    |Total|Total size of data|

    |**Basic Data**|**Description**|
    |CDN Inbound|Data uploaded from local to OSS through CDN service layer|
    |CDN Outbound|Data downloaded from OSS through CDN service layer|
    |Internet Inbound|Data uploaded from local to OSS through Internet|
    |Internet Outbound|Data downloaded from OSS to local through Internet|
    |Intranet Inbound|Data uploaded from ECS servers to OSS through Alibaba intranet|
    |Intranet Outbound|Data downloaded from OSS to ECS servers through Alibaba intranet|
    |Cross-Region Replication Inbound|Data synchronously replicated from the target bucket to the source bucket using the cross-region replication function|
    |Cross-Region Replication Outbound|Data synchronously replicated from the source bucket to the target bucket using the cross-region replication function|

    |**Basic Data**|**Description**|
    |GET Request|Number of GET requests per hour|
    |PUT Request|Number of PUT requests per hour|

4.  Select the time granularity of the resource usage diagrams, as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4766/2297_en-US.png)

    -   **Today**: Only the data of the current day is shown in diagrams.
    -   **Yesterday**: Only the data of yesterday is shown in diagrams.
    -   **7 Days**: Only the data of the latest seven days is shown in diagrams.
    -   Customized time period: You can select the Start Date and the End Date of a time period. The data in this period is shown in diagrams.
5.  Check the required basic data in the corresponding diagram. The following part uses the Bucket diagram as an example to describe the checking method of basic data.
    -   The display status of a basic data item is shown on the lower right of a diagram. If the circle before a basic data item is empty, the basic data item is not shown in the diagram. If the circle before a basic data item is solid, the basic data item is shown in the diagram.

        For example, in the following figure, the **Standard** and **Archive** data items are not shown in the diagram, and the **Infrequent Access** and **Total** data items are shown in the diagram.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4766/2298_en-US.png)

        **Note:** All data items are shown in diagrams by default.

    -   By clicking the circle before a basic data item, you can switch between the following status: 1. Show the basic data item in the diagram. 2. Do not show the basic data item in the diagram.
    -   By double-clicking the circle before a basic data item, you can switch between the following two status: 1. Only show this basic data item in the diagram. 2. Show all basic data items in the diagram.

