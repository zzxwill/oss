# Data migration {#concept_ydb_yjh_wdb .concept}

This article mainly introduces the general application of OssImport and implementation of typical requirements.

## Introduction to OssImport {#section_skm_zjh_wdb .section}

Deployment mode

The OssImport has two deployment modes available: standalone mode and distributed mode. 

-   For small-scale data migration with the data size smaller than 30 TB, the standalone mode is enough.
-   Distributed mode is recommended for migration of a large data size.

Time-specific traffic limits

OssImport is based on master-worker distributed architecture. The worker offers a traffic limit feature. You can implement traffic limits through modifying the `workerMaxThroughput(KB/s)` item in the configuration file `sys.properties`. This configuration item does not take effect. Restart the service after the modification for the item to take effect.

In distributed deployment mode, modify the sys.properties in $OSS\_IMPORT\_WORK\_DIR/conf for each worker and then restart the service.

You can use `crontab` to implement timed modification of `sys.properties`, and then restart the service to implement time-specific traffic limits.

Add a worker

Determine the worker list before submitting the job. Currently OSS does not support adding workers dynamically.

Data validation without migration

The OssImport supports data validation without migration. In the job configuration file  `job.cfg` or `local_job.cfg`, the configuration item `jobType` is `audit` instead of `import`. Other configuration items are the same as data migration.

Incremental mode of data migration

The incremental mode of data migration refers to the process of performing a full migration first after a data migration job is started and then performing incremental migration operations at set intervals automatically. The first data migration job is a full migration. The job is started immediately after it is submitted. The subsequent data migration jobs are initiated once every set interval. The incremental mode of data migration applies to data backup and data synchronization.

The incremental mode has two configuration items:

-   In job. cfg, `isIncremental`  indicates whether incremental migration mode is turned on. `true` means that the incremental mode is enabled; `false` means that the incremental mode is disabled. The default value is false.
-   In job. cfg, `incrementalModeInterval` specifies the synchronization interval in seconds for the incremental mode. `The setting is used` when isIncremental is set to true. The minimum value configurable is `900 seconds`. We do not recommend that you configure it to a value smaller than `3600 seconds`, because it wastes a large number of requests and lead to additional system overhead.

Specify filtering conditions for object migration

Only objects that meet the specified filtering conditions are migrated. The OssImport supports specifying the `prefix` and `last modified time` :

-   The `srcPrefix` setting in job.cfg specifies the prefix of the objects to be migrated. It is empty by default.
    -   If the `srcType=local`, enter the local directory in full path, and separate the input values with `/` and end the input values with `/`, such as `c:/example/` or `/data/example/`.
    -   If the `srcType`  is `oss`, `qiniu`, `bos`, `ks3`, `youpai`, or `s3`, enter the prefix of the objects to be synchronized, excluding the bucket name, such as `data/to/oss/`. The `srcPrefix` of all objects must be set to empty.
-   In job.cfg, the `importSince` option specifies the last modified time of the migration objects. It is an integer and expressed in seconds. `The importSince setting` is in the Unix timestamp format, that is, the number of seconds since 00:00 UTC on January 1, 1970. You can get the value through the `date  +%s` command. The default value is 0, indicating to migrate all the data. The incremental mode of data migration is only valid for the first full migration. The non-incremental mode is valid for the entire migration job.
    -   If an object’s `LastModified Time` is at or before `importSince`, it is migrated.
    -   If an object’s `LastModified Time` is after  `importSince`, it is not migrated. 

## Typical scenarios {#section_qrc_1kh_wdb .section}

Seamlessly switch from a third-party storage service to the OSS

Follow these steps, you can switch from other storage services to the OSS seamlessly:

1.  Full migration. At this point, the business is still running on the third-party storage service. Mark down the start time of the data migration T1. Note that the time must be in the Unix timestamp format, that is, the number of seconds since 00:00 UTC on January 1, 1970. You can get the value through the `date +%s` command.
2.  Open the OSS image origin retrieval feature. After the data migration is complete, set the [image origin retrieval](../../../../intl.en-US/Developer Guide/Manage files/Manage origin retrieval settings.md#) feature for the bucket in the OSS console, and the origin retrieval address is the third-party storage.
3.  Switch reading/writing to the OSS. At this point, the data earlier than T1 is read from the OSS, while the data later than T1 is read from the third-party service using the image origin retrieval, and new data is fully written to the OSS.
4.  Incremental data migration. In the configuration file \(`job.cfg`  or `local_job.cfg`\),  the configuration item for an incremental migration job is `importSince=T1`. The incremental migration is completed at T2.

    **Note:** Incremental data migration is not an incremental mode of data migration.

5.  Delete the third-party storage. After T2, all your business reads and writes occur on the OSS, and the third-party storage is only a copy of historical data. You can decide to keep it or remove it at your own discretion. The OssImport is responsible for data migration and validation. It does not delete any data.

Migrate local data to the OSS

ools for migrating local data to the OSS:

-   If you want to migrate less than 30 TB of local data files, or want to mount the storage service to a local file system, we recommend that you use [OssUtil](intl.en-US/Utilities/ossutil/Download and installation.md#). The tool is easy and convenient to use. OssUtil supports incremental uploads at the object level and implements the feature through the `-u/--update` and `--snapshot-path` options. For detailed descriptions, run the `ossutil help cp` command to see details.
-   The distributed version of [OssImport](intl.en-US/Utilities/ossimport/Distributed deployment.md#) is recommended for migration of large scale data.

    **Note:** During incremental migration of local data, some operations of the file system won’t modify the last modified time of objects, such as cp and mv in Windows, and mv and rsync with `-t` or `-a` options in Linux. Data changes from these operations are not detected or synchronized to the OSS.


Data migration between OSS

-   When to use OssImport:
    -   If you want to add the *Cross-Region Replication* feature for data migration between OSS in different regions, you can configure the feature in the console.
    -   If a region does not support *Cross-Region Replication* yet for security reasons, you can use OssImport to migrate or back up data.
    -   Data migration between different accounts and buckets within the same region.
    -   We recommend Alibaba Cloud intranet for direct data migration within OSS, that is, using the ECS or OSS domain name with `internal`.
-   Charges for direct data migration within OSS:
    -   If you use a domain name with `internal`, no traffic charges incur, but must pay for the request and storage charges.
    -   If you did not use a domain name with `internal`, traffic charges may be incurred.
-   Not recommended use cases:
    -   Data migration between regions with the Cross-Region Replication service activated.
    -   When you synchronize modifications to objects between OSS in incremental mode, the OssImport only supports synchronization of object modifications \(put/append/multipart\) and does not support synchronizing reading and deleting operations. The data synchronization is not guaranteed to be timely by a specific SLA. Exert caution when selecting this option. We recommend that you use [Upload callback](../../../../intl.en-US/Developer Guide/Upload files/Upload callback.md#).

## Migration instructions {#section_f15_1kh_wdb .section}

ECS and traffic

If you want to migrate data from the cloud \(non-local\) to the OSS and have insufficient bandwidth resources, we recommend that you buy Pay-As-You-Go ECS instances for the migration. ECS configuration:

-   Select Pay-As-You-Go for the billing method.
-   Select the corresponding region for the OSS.
-   Select 100 MB for the bandwidth peak.

In migration job configuration, set the `targetDomain` to an intranet domain name containing `internal`. If the source end is also OSS, also set the `srcDomain` to an intranet domain name containing `internal`. This saves money for downloads from the OSS source domain name, and only charges for OSS access.

Migrate HTTP data to OSS

Parameters to be configured for an HTTP data migration job:

-   In job.cfg, set `srcType`  to `srcType=http`. It is case-sensitive.
-   In `httpListFilePath` of job.cfg, use absolute paths to specify the HTTP address list file, such as `c:/example/http.list`、`/root/example/http.list` . A full HTTP link is `127.0.0.1/aa/bb.jpg`. Different splitting methods may lead to different object paths on the OSS after the upload:

    ```
    http://127.0.0.1/aa/   bb.jpg      #  The first line
      http://127.0.0.1/      aa/bb.jpg   # The second line
    ```

    The object name after the first line is imported to the OSS is `destPrefix + bb.jpg` and the object name of the second line is `destPrefix + aa/bb.jpg`. The httpPrefixColumn parameter specifies the domain name column. The first column applies by default, such as the aforementioned `127.0.0.1/aa/`  or `127.0.0.1/`. The relativePathColumn specifies the object name in the OSS, such as the aforementioned `bb.jpg` or `aa/bb.jpg`. If the object has multiple columns, as follows:

    ```
    http://127.0.0.1/aa/   bb/cc dd/ee  ff.jpg
    ```

    The configuration must be: httpPrefixColumn=1 , relativePathColumn=4

-   The `destAccessKey`,  `destSecretKey`,  `destDomain`, and `destBucket` configuration items among others in job.cfg.

Splitting parameters for HTTP data migration tasks:

-   `taskObjectCountLimit`: The maximum number of objects for each task. The default value is 10,000.
-   `taskObjectSizeLimit` : The maximum data size of each task. This parameter is invalid for HTTP data migration, because when the master is splitting tasks, if every HTTP object is the size of the object obtained from the source, each object has one HTTP request overhead, which negatively impacts the task allocation efficiency, thereby compromising concurrent execution of tasks and migration efficiency.
-   `Domain name`: The first column in the object specified by `httpListFilePath`. Continuous jobs with the same domain name are split according to the `taskObjectCountLimit` parameter, and continuous jobs with different domain names are split into different tasks to make better reuse of connections. For example:

    ```
    http://mingdi-hz.oss-cn-hangzhou.aliyuncs.com/ import/test1.txt
      http://mingdi-hz.oss-cn-hangzhou.aliyuncs.com/ import/test2.txt
      http://mingdi-bj.oss-cn-beijing.aliyuncs.com/ import/test3.txt
      http://mingdi-bj.oss-cn-beijing.aliyuncs.com/ import/test4.txt
    ```

    `taskObjectCountLimit` When the taskObjectCountLimit value is greater than 2, the job is split into two tasks, while in the following conditions, the job is split into four tasks.

    ```
    http://mingdi-hz.oss-cn-hangzhou.aliyuncs.com/ import/test1.txt
      http://mingdi-bj.oss-cn-beijing.aliyuncs.com/ import/test3.txt
      http://mingdi-hz.oss-cn-hangzhou.aliyuncs.com/ import/test2.txt
      http://mingdi-bj.oss-cn-beijing.aliyuncs.com/ import/test4.txt
    ```

    That is why `httpListFilePath`  specified HTTP address list objects are first sorted by domain name.


Network traffic and parameter configuration

The configuration of the following parameters is related to network traffic:

-   In sys.properties, the `workerTaskThreadNum` parameter indicates the number of jobs for concurrent execution by the worker. If the network quality is poor or the concurrency is high, there may be a large number of time-out errors. At this point, we recommend that you reduce the concurrency, modify the configuration item and restart the service.
-   In sys.properties, the `workerMaxThroughput(KB/s)` parameter indicates the traffic ceiling of the worker. If you want to limit the traffic, such as for throttling on the source end, or out of network restrictions, the value of this parameter must be smaller than the maximum network traffic allowed for the machine and evaluated based on business requirements.
-   In job.cfg, the `taskObjectCountLimit` parameter indicates the maximum number of objects of each task. The default value is 10,000. This parameter influences the number of tasks. If the number of tasks is too small, the concurrent tasks may be less efficient.
-   In job.cfg, the `taskObjectSizeLimit` indicates the maximum data size of each task. The default value is 1 GB. This parameter influences the number of tasks. If the number of tasks is too small, the concurrent tasks may be less efficient.

    **Note:** 

    -   We recommend that you determine the configuration file parameters before starting the migration.
    -   Modifications to parameters in sys.properties take effect after you restart the migration server.
    -   After the job.cfg job is submitted, the configuration parameters of the job cannot be changed.

