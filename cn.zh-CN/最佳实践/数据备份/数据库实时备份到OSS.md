# 数据库实时备份到OSS {#concept_akc_b4t_42b .concept}

本文介绍如何通过数据库备份DBS将本地IDC、公网、第三方云数据库、阿里云RDS和阿里云ECS自建数据库实时备份到OSS上。

## 背景 {#section_r5y_k4t_42b .section}

-   对象存储OSS

    [对象存储OSS](https://www.aliyun.com/product/oss)提供了标准类型存储，作为移动应用、大型网站、图片分享或热点音视频的主要存储方式，也提供了成本更低、存储期限更长的低频访问类型存储和归档类型存储，作为不经常访问数据的备份和归档。对象存储OSS非常适合作为数据库备份的存储介质。

-   数据库备份DBS

    [数据库备份DBS](https://www.aliyun.com/product/dbs)是为数据库提供连续数据保护、低成本的备份服务。它可以为多种环境的数据提供强有力的保护，包括企业数据中心、其他云厂商及公共云。数据库备份提供数据备份和操作恢复的整体方案，具备实时增量备份、精确到秒级的数据恢复能力。


## 应用场景 {#section_zxt_54t_42b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16444/15325941077487_zh-CN.png)

数据库备份到OSS的方案应用场景如下：

-   阿里云RDS或阿里云ECS自建数据库在OSS上备份或长期归档

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16444/15325941077610_zh-CN.jpg)

-   自建IDC数据备份上云

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16444/15325941077660_zh-CN.png)

-   其他云数据库在阿里云OSS上做多云备份

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16444/15325941077661_zh-CN.png)

-   数据库做异地备份灾备

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16444/15325941077663_zh-CN.png)


## 方案优势 {#section_ivm_ppt_42b .section}

-   支持全量或实时增量备份

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16444/15325941077488_zh-CN.png)

    -   秒级RPO：日志内存实时捕获，CDP实时备份，RPO达到秒级。
    -   无锁并发：全程无锁备份、并发备份、数据拉取自适应分片。
    -   精准恢复：恢复对象精准匹配，单表恢复，从而大幅降低RTO。
    -   灵活恢复：提供可恢复日历及时间轴选择，实现任意时间点恢复。
-   数据强安全高可靠

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16444/15325941077668_zh-CN.png)

    -   异地灾备：利用OSS的跨区域复制功能，做备份数据的异地灾备，提升数据保护级别。
    -   数据传输加密：在传输过程中进行SSL加密，保障数据安全性。
    -   数据存储加密：对备份到OSS的数据进行加密存储，保障数据隐私性。
    -   随时验证：随时验证数据库备份的有效性。
-   低成本

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16444/15325941077670_zh-CN.png)

    -   按需付费：OSS存储空间按需付费，避免一次性投入大量资产。
    -   自动存储分级：OSS提供标准、低频、归档多种类型，全面优化存储成本。
    -   弹性扩展：OSS存储容量弹性扩展，无缝支撑企业在不同发展阶段的性能要求。
-   支持多环境多数据源

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16444/15325941087674_zh-CN.png)

    -   支持MySQL、Oracle、SQL Server、MongoDB多种数据库。
    -   支持IDC、第三方云数据库、阿里云RDS、阿里云ECS自建数据库。

## 方案实施流程 {#section_mjr_qst_42b .section}

数据库备份到OSS的方案实施流程如下：

1.  创建备份计划。详情请参见*数据库备份快速入门*中的[创建备份计划](https://help.aliyun.com/document_detail/65909.html)。
2.  配置备份计划。详情请参见*数据库备份快速入门*中的[配置备份计划](https://help.aliyun.com/document_detail/59609.html)。
3.  查看备份计划。详情请参见*数据库备份快速入门*中的[查看备份计划](https://help.aliyun.com/document_detail/59616.html)。
4.  恢复数据库。详情请参见*数据库备份快速入门*中的[恢复数据库](https://help.aliyun.com/document_detail/85543.html)。

