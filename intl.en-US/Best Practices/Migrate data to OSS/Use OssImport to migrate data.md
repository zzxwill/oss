# Use OssImport to migrate data {#concept_awy_rdg_vdb .concept}

This topic describes how to migrate data from third-party storage products \(or from another OSS source\) to OSS by using OssImport.

## Environment configuration {#section_mj4_4s4_fgb .section}

OssImport can be deployed in two modes: standalone mode and distributed mode.

-   Standalone mode applies to small-scale data migration scenarios where the data size is smaller than 30 TB.
-   Distributed mode applies to large-scale data migration scenarios.

For example, you need to migrate 500 TB of data from an AWS S3 bucket in the Tokyo region to an OSS bucket in the China East 1 \(Hangzhou\) region within a week. Before migrating the data, you must configure environments to deploy OssImport in distributed mode as follows:

-   Activate OSS.
    1.  Use your Alibaba Cloud account to create an OSS bucket in China East 1 \(Hangzhou\).
    2.  Create a RAM user in the RAM console, and then grant OSS access permissions to the RAM user. You also need to securely store the AccessKeyID and AccessKeySecret of the RAM user.
-   Purchase ECS instances.

    Purchase ECS instances with two CPUs and 4 GB of memory in the China East 1 \(Hangzhou\) region \(in which the OSS bucket is also created\). If you want to release the ECS instances after data migration, we recommend that you select Pay-As-You-Go as the billing method when purchasing the instances.

    The number of required ECS instances can be calculated as follows: X/Y/\(Z/100\). In the formula, X indicates the size of data that needs to be migrated, Y indicates the number of days that the migration requires, and Z indicates the transfer speed of a single ECS instance \(MB/s\), that is, how much TB of data can be migrated by a single ECS instance each day \(calculated as Z/100\). Assume that the transfer speed of an ECS instance is 200 MB/s \(that is, an ECS instance can migrate 2 TB of data each day\). This means you must purchase 36 ECS instances in total \(calculated from 500/7/2\).

-   Configure OssImport

    For the large-scale data migration requirement in this example, you must deployed OssImport in ECS instances in distributed mode. For the configuration information about distributed mode, such as `conf/job.cfg`, `conf/sys.properties`, and concurrency control, see [Architecture and configuration](../../../../../reseller.en-US/Tools/ossimport/Architecture and configuration.md#). For more information about OssImport distributed deployment, such as the downloading method of OssImport and the troubleshooting of OssImport configuration, see [Distributed deployment](../../../../../reseller.en-US/Tools/ossimport/Distributed deployment.md#).


## Procedure {#section_bdl_ybq_fgb .section}

You can use OssImport in distributed mode to migrate data from AWS S3 to OSS as follows:

**Note:** After deploying OssImport in distributed mode in the ECS instances, use OssImport to download data from the AWS S3 bucket in the Tokyo region to the ECS instances in China East 1 \(Hangzhou\). We recommend you download the data through Internet. Use OssImport to upload the data from the ECS instances to the OSS bucket in China East 1 \(Hangzhou\). We recommend you upload the data through the intranet.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4431/15490913121976_en-US.png)

1.  Fully migrate the historical data in AWS S3 before the time T1. For more information, see the Running section in [Distributed deployment](../../../../../reseller.en-US/Tools/ossimport/Distributed deployment.md#section_m1y_1jh_wdb).

    T1 is a UNIX timestamp, namely, the number of seconds elapsed since UTC 00:00, January 1, 1970, and can be obtained by running the `date +%s` command\).

2.  In the OSS console, enable [Back-to-Origin](../../../../../reseller.en-US/Console User Guide/Manage buckets/Set back-to-origin rules.md#) for the target bucket and set the access URL of the AWS S3 as the origin URL.
3.  Switch all read and write operations on AWS S3 to OSS, and record the time \(T2\).

    In this way, all historical data before T1 is directly read from the OSS bucket, and the data stored between T1 and T2 is read from AWS S3 through the mirroring back-to-origin function of OSS.

    After T2, all new data is written to OSS and no new data is written to AWS S3.

4.  Modify the item `importSince=T1` in the configuration file `job.cfg`, and then start a migration task again to migrate the data added between T1 and T2.

    **Note:** 

    -   After step 4, all read and write operations are performed on the target OSS bucket. Data stored in AWS S3 is historical data, which can be retained or deleted as needed.
    -   OssImport only migrates and verifies data but does not delete it.

Various costs are incurred during data migration, including the cost of ECS instances, traffic costs, storage costs, and time-dependent costs. Furthermore, if the size of the data to be migrated is larger than 1 TB, the storage cost increases due to the time required for the migration. However, the storage cost generally remains lower than the costs associated with network traffic and ECS instances. You can reduce the time needed for migration by using more ECS instances.

## References {#section_fvm_cbv_fgb .section}

For more information about OssImport, see the following documentations:

[Distributed deployment](../../../../../reseller.en-US/Tools/ossimport/Distributed deployment.md#)

[Architecture and configuration](../../../../../reseller.en-US/Tools/ossimport/Architecture and configuration.md#)

[Data migration](../../../../../reseller.en-US/Tools/ossimport/Data migration.md#)

[FAQ](../../../../../reseller.en-US/Tools/ossimport/FAQ.md#)

