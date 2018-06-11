# Use OssImport to migrate data {#concept_awy_rdg_vdb .concept}

This article explains how to migrate data from third-party storages \(or OSS\) to OSS by using OssImport.

## Deployment mode: standalone and distributed  {#section_pnx_ydg_vdb .section}

OssImport supports standalone and distributed deployment modes.  Generally, the distributed mode is recommended.  This article provides the information you may want to know  and the documentation resources you may find useful throughout the migration process.

## Migration solution \(from a third-party storage to OSS\) {#section_dh2_12g_vdb .section}

Follow these steps to seamlessly migrate data from a third-party storage to OSS: \(See [OssImport architecture and configuration](../intl.en-US/Utilities/ossimport/Architecture and configuration.md#) for officially supported third-party storage types\)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4431/1976_en-US.png)

The detailed steps are as follows:

1.  Step 1: Fully migrate the historical data before T1. See [OssImport architecture and configuration](../intl.en-US/Utilities/ossimport/Architecture and configuration.md#).
    -   Record the migration start time T1 \(which is a UNIX timestamp, namely, the number of seconds elapsed since UTC 00:00, January 1, 1970, and is retrieved by the `date +%s`command\).
    -   See the [OssImport distributed deployment](../intl.en-US/Utilities/ossimport/Distributed deployment.md#) migration instructions.
2.  Step 2: Enable OSS back-to-source and switch read/write to OSS, so that no new data is added to the migration source.
    -   After Step 1 is complete, enable the [OSS back-to-source function](../intl.en-US/Console User Guide/Manage buckets/Set back-to-origin rules.md#) in the OSS console, where the source is the migration source \(the third-party storage\).
    -   When the read/write of the business system is switched to OSS, assume that this point of time is T2.
    -   In this case, data before T1 is read from OSS, data after T1 is read from the third-party service by OSS through the back-to-source function, and new data is fully written to OSS.
3.  Step 3: Quickly migrate the data generated between T1 and T2.
    -   After Step 2 is complete, no new data is added to the third-party storage, and data read/write is switched to OSS.
    -   Modify the configuration items `importSince=T1` in the configuration file `job.cfg` to initiate a new migration job for migrating the data generated between T1 and T2.
4.  Complete the migration: After Step 3 is complete, you have finished the migration.
    -   After Step 3 is complete, all reads/writes of your business are switched to OSS. The third-party storage maintains only the historical data, which can either be retained or deleted as needed.
    -   OssImport migrates and verifies data but does not delete any data.

Migration costs

Migration costs include the ECS, traffic, storage, and time costs.  In most cases, such as TB-level data migration, the storage cost is directly proportional to the migration cost, and the ECS cost is lower than the traffic and storage costs.

Prerequisites

Assume that you have the following migration requirements:

|Item|Description|
|:---|:----------|
|Migration source|AWS S3 \(Tokyo\)|
|Migration target|OSS \(Hong Kong\)|
|Data volume|500 TB|
|Required migration schedule|Within one week|

Prepare an environment as follows:

|Item|Description|
|:---|:----------|
|Activate OSS|The steps for activating OSS are as follows:1.  Create an OSS \(Hong Kong\) bucket with your account.
2.  Create a new sub-account in the RAM console and grant the sub-account the permission to access OSS.  Save the AccessKeyID and AccessKeySecret.

|
|Purchase ECSs|Purchase ECSs that reside in the same region \(Hong Kong\) as OSS. Typically, two CPU cores and 4 GB memory are the recommended configurations. If the ECSs are to be released after the migration, determine the configurations as needed. For the number of ECSs during the actual migration, see [Number of ECSs](#ecs1).|
|Configure OssImport| **Note:** Use an intranet OSS endpoint for the target endpoint of the destDomain \([OssImport architecture and configuration](../intl.en-US/Utilities/ossimport/Architecture and configuration.md#)\). The ECS-OSS communication occur over the intranet, which does not incur any Internet traffic costs.  For more information, see [Regions and endpoints](../intl.en-US/Developer Guide/Regions and endpoints.md#).

 |
|Migration procedure| 1.  Build an OssImport distributed environment on the ECS.
2.  OssImport downloads data from AWS S3 \(Tokyo\) to the ECS \(Hong Kong\) over the Internet.
3.  OssImport uploads data from the ECS \(Hong Kong\) to the OSS \(Hong Kong\) over the intranet.

 |

The number of ECSs for migration

Calculate the required number of ECSs for migration as needed:

-   Assume that you want to migrate X TBs of data within Y days at a speed of Z Mbps per ECS. \(About Z/100 TBs of data is migrated every day.\)

-   For the actual migration, approximately X/Y/\(Z/100\) ECSs are required.


Assume that the migration speed per ECS is 200 Mbps.\(About 2 TBs of data is migrated every day.\)   In this case, approximately 36 ECSs \(500/7/2\) are required.

## Migration procedure using OssImport {#section_rcn_f3g_vdb .section}

Configuration reference

Read the tutorials [OssImport  architecture and configuration](../intl.en-US/Utilities/ossimport/Architecture and configuration.md#) and [Distributed deployment](../intl.en-US/Utilities/ossimport/Distributed deployment.md#), respectively, for the configuration definition and the procedure. Check the following information before getting started:

-   OssImport downloading: Download the distributed version of OssImport on the master and use the same SSH account and password for the master and worker. \(You do not have to download OssImport to the worker.  OssImport is automatically distributed to the worker when the `deploy` command is executed. For more information, see [Distributed deployment](../intl.en-US/Utilities/ossimport/Distributed deployment.md#).\)

-   Java environment: Make sure that both the master and worker are installed with Java.

    **Note:** The worker must be installed with Java as well.

-   workdir configuration: Specify a workdir by configuring the configuration item `workingDir` in `conf/sys.properties`. For more information, see [Distributed deployment](../intl.en-US/Utilities/ossimport/Distributed deployment.md#).

    **Note:** See the relevant documentation for how to configure the workdir. Do not set it as the path to the OssImport package, or, wherever possible, another non-empty directory.

-   Concurrency control: Configure the configuration item `taskObjectCountLimit` in `conf/job.cfg` and `workerTaskThreadNum` in `conf/sys.properties`. For more information, see [OssImport architecture and configuration](../intl.en-US/Utilities/ossimport/Architecture and configuration.md#).

    If most of the files to be migrated are small files and both the migration speed of each ECS and  the CPU workload are low, consider increasing `workerTaskThreadNum` and decreasing `taskObjectCountLimit` and see how it works out.

-   Other issues encountered during the operation are generally associated with configuration. For more information, see [Distributed deployment](../intl.en-US/Utilities/ossimport/Distributed deployment.md#) and the log files under `workdir/Logs` on the master and worker.

Test run

We recommend that you build a small environment \(such as with two or three ECSs\) and migrate a small amount of data to verify if the configuration is proper.  Verify that the migration bandwidth of each ECS reaches the expected value such as 200 Mbps. If you do not have any specific requirement on the migration schedule, skip this verification item.

View the bandwidth: Use `iftop` or `Nload` \(which needs to be installed first by running the `yum install ***` command\) for this purpose, because latency exists in the bandwidth statistics in the ECS console.

**Note:** You may not be able to verify the concurrency if the number of testing files is low. In this case, decrease `taskObjectCountLimit` \(such as to the number of files/workerTaskThreadNum/total number of workers\) and see how it works out.

## Migration procedure {#section_hdz_pkg_vdb .section}

1.  Migrate historical data.
    -   Clear tasks and configurations.
    -   See [Distributed deployment](../intl.en-US/Utilities/ossimport/Distributed deployment.md#) for the specific procedure.
    -   Start the migration.
    -   You can view the actually migrated data in the OSS bucket in the [OSS console](https://oss.console.aliyun.com/overview) .
2.  Configure OSS back-to-source and switch the read/write of the business system to OSS.
    -   Enable [OSS back-to-source](../intl.en-US/Console User Guide/Manage buckets/Set back-to-origin rules.md#) in the [OSS console](https://oss.console.aliyun.com/), where the back-to-source address is the migration source \(the third-party storage\).
    -   Assume that the point of time when the read/write of the business system is switched to OSS is T2. Then no new data is written to the migration source after T2.
3.  Migrate remaining data.

    Modify the configuration item `importSince=T1` in `job.cfg`. See [OssImport  architecture and configuration](../intl.en-US/Utilities/ossimport/Architecture and configuration.md#). Migrate the remaining data \(between T1 and T2\).In special cases, you can migrate again by setting  `importSince = 0, isSkipExistFile=true` in `job.cfg`. For more information, see [OssImport architecture and configuration](../intl.en-US/Utilities/ossimport/Architecture and configuration.md#).


The migration speed

-   Migration speed per ECS: If the migration speed of each ECS is low \(for example, the speed is lower than 200 Mbps and the CPU  workload is low\), you can optimize the concurrency control parameters by referring to relevant tutorials and this article, that is, modify `taskObjectCountLimit` in `conf/job.cfg` and `workerTaskThreadNum` in `conf/sys.properties` and see how it works out.

-   Number of ECSs for migration: See [Number of ECSs](#ecs1) to estimate the required number of ECSs. \(The ECS cost only accounts for a minor part of the total migration costs compared with the traffic, storage, and time costs.\)  The migration is accelerated when more ECSs are in place.


## Tutorials related to OssImport distributed deployment {#section_whk_ykg_vdb .section}

|No.|Tutorial|
|:--|:-------|
|1|[OssImport distributed deployment](../intl.en-US/Utilities/ossimport/Distributed deployment.md#)|
|2|[OssImport architecture and configuration](../intl.en-US/Utilities/ossimport/Architecture and configuration.md#)|
|3|[OssImport best practices](../intl.en-US/Utilities/ossimport/Data migration.md#)|
|4|[OssImport FAQ](../intl.en-US/Utilities/ossimport/FAQs.md#)|

