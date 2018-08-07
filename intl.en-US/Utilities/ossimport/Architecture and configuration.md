# Architecture and configuration {#concept_rc2_t1h_wdb .concept}

## Overview {#section_cf5_t1h_wdb .section}

The OssImport tool allows you to migrate data stored locally or in other cloud storage systems to the OSS. It has the following features:

-   Supports a rich variety of data sources including local drives, Qiniu, Baidu BOS, AWS S3, Azure Blob, Youpai Cloud, Tencent Cloud COS, Kingsoft KS3, HTTP, and OSS, and can be expanded as needed.
-   Supports resumable data transfers.
-   Supports throttling.
-   Supports migrating objects after a specified time point or with a specified prefix.
-   Supports parallel data uploads and downloads.
-   Supports standalone and distributed modes. The standalone mode is easy to deploy and use, and the distributed mode is suitable for large-scale data migration.

## Environment {#section_cwg_v1h_wdb .section}

-   Java 1.7 and later

## Architecture {#section_ttn_x1h_wdb .section}

The OssImport has two deployment modes available: standalone mode and distributed mode.

-   The standalone mode is sufficient for small-scale data migration which smaller than 30 TB. [Download](http://gosspublic.alicdn.com/ossimport/standalone/ossimport-2.3.1.zip?spm=a2c4g.11186623.2.4.tg6Ory&file=ossimport-2.3.1.zip)
-   Distributed mode is recommended for larger data migrations. [Download](http://gosspublic.alicdn.com/ossimport/distributed/ossimport-2.3.1.tar.gz?spm=a2c4g.11186623.2.4.maVna1&file=ossimport-2.3.1.tar.gz)

-   Standalone

    The master, worker, tracker, and console run on the same machine. There is only one worker in the system. We have encapsulated and optimized the deployment and execution of the standalone mode and the standalone deployment and execution are both easy. In standalone mode, the master, worker, tasktracker, and console modules are packaged into `ossimport2.jar`.

    The file structure in standalone mode is as follows:

    ```
    ossimport
    ├── bin
    │ └── ossimport2.jar  # The JAR including master, worker, tracker, and console modules
    ├── conf
    │ ├── local_job.cfg   # Standalone job configuration file
    │ └── sys.properties  # Configuration file for the system running
    ├── console.bat         # Windows command line, which can run distributed call-in tasks
    ├── console.sh          # Linux command line, which can run distributed call-in tasks
    ├── import.bat          # The configuration file for one-click import and execution in Windows is the data migration job configured in conf/local_job.cfg, including start, migration, validation, and retry
    ├── import.sh           # The configuration file of one-click import and execution in Linux is the data migration job configured in conf/local_job.cfg, including start, migration, validation, and retry
    ├── logs                # Log directory
    └── README.md           # Description documentation. We recommend that you carefully read the documentation before using this feature
    ```

    Note:

    -   The import.bat or import.sh file is a one-click import script and can be run directly after you complete modification to `local_job.cfg`.
    -   The console.bat or console.sh is the command line tool and can be used for distributed execution of commands.
    -   Run scripts or commands in the `ossimport` directory, that is, the directory at the same level as the `*.bat/*.sh` file.
-   Distributed

    The OssImport is based on the master-worker distributed architecture, as shown in the following figure:

    ```
    Master --------- Job --------- Console
        |
        |
       TaskTracker
        |_____________________
        |Task     | Task      | Task
        |         |           |
    Worker      Worker      Worker
    ```

    In the figure:

    -   Job: The data migration jobs submitted by users. For users, one job corresponds to one configuration file `job.cfg`.
    -   Task: A job can be divided into multiple tasks by data size and number of files. Each task migrates a portion of files. The minimal unit for dividing a job into tasks is a file. One file cannot be split into multiple tasks.
    The OssImport tool modules are listed in the following table:

    |Role|Description|
    |:---|:----------|
    |Master|The master is responsible for splitting a job into multiple tasks by data size and number of files. The data size and number of files can be configured in sys.properties. The detailed process for splitting a job into multiple tasks is as follows:    1.  The master node scans the full list of files to be migrated from the local/other cloud storage devices.
    2.  The master splits the full file list into tasks by data size and the number of files and each task is responsible for the migration or validation for a part of files.
 |
    |Worker|     -   The worker is responsible for file migration and data validation of tasks. It pulls the specific file from the data source and uploads the file to the specified directory to the OSS. You can specify the data source to be migrated and the OSS configuration in job.cfg or local\_job.cfg.
    -   Worker data migration supports limiting traffic and specifying the number of concurrent tasks. You can configure the settings in sys.properties.
 |
    |TaskTracker|TaskTracker is abbreviated to Tracker. It is responsible for distributing tasks and tracking task statuses.|
    |Console|The console is responsible for interacting with users and receiving command display results. It supports system management commands such as deploy, start, and stop, and job management commands such as submit, retry, and clean.|

    In distributed mode, you can start multiple worker nodes for data migration. Tasks are evenly allocated to the worker nodes and one worker node can run multiple tasks. One machine can only start one worker node. The master is started at the same time as the first worker node configured in `workers`, and the tasktracker and console also run on the machine.

    The file structure in distributed mode is as follows:

    ```
    ossimport
    Miss -- Bin
    │ ├── console.jar     # The JAR package of the console module
    │ ├── master.jar      # The JAR package of the master module
    │ ├── tracker.jar     # The JAR package of the tracker module
    │ └── worker.jar      # The JAR package of the worker module
    ├── conf
    │ ├── job.cfg         # The template of the job configuration file
    │ ├── sys.properties  # Configuration file for the system running parameters
    │ └── workers         # Worker list
    ├── console.sh          # The command line tool. Currently it only supports Linux
    ├── logs                # Log directory
    └── README.md           # Description documentation. We recommend that you carefully read the documentation before using the feature
    ```

    Note:

    -   The distributed command line tool console.sh currently only supports Linux and does not support Windows.

## Configuration files {#section_c2z_ldh_wdb .section}

In standalone mode, two configuration files are used: `sys.properties` and `local_job.cfg`. In distributed mode, three configuration files are used: `sys.properties`, `local_job.cfg`, and `workers`. Specifically, `local_job.cfg` and `job.cfg` are identical, except in name. The `workers` file is exclusive to the distributed environment. 

-   sys.properties

    System running parameters.

    |Field|Meaning|Description|
    |:----|:------|:----------|
    |workingDir|Working directory.|     -   The directory after the tool kit is extracted. 
    -   Do not modify this option in standalone mode.
    -   The working directory for each machine in distributed mode must be the same.
 |
    |workerUser|The worker machine SSH user name.|     -   If you have configured privateKeyFile, the privateKeyFile is used in priority.
    -   If privateKeyFile is not configured, the workerUser/workerPassword combination is used.
    -   Do not modify this option in standalone mode.
 |
    |workerPassword|The worker machine SSH user password.|Do not modify this option in standalone mode.|
    |privateKeyFile|The file path of the private key.|     -   If you have established an SSH channel, you can specify the public key file path. Otherwise, leave it empty. 
    -   If you have configured privateKeyFile, the privateKeyFile is given priority.
    -   If privateKeyFile is not configured, the workerUser/workerPassword is used.
    -   Do not modify this option in the standalone mode.
 |
    |sshPort|The SSH port.|The default value is 22. It does not usually need to be changed. Do not modify this option in standalone mode.|
    |workerTaskThreadNum|The maximum number of threads for the worker to run tasks.|     -   This parameter is related to the machine memory and network. Recommended value is 60.
    -   The value can be increased, for example to 150 for physical machines. If the network bandwidth is already full, do not increase the value further.
    -   If the network is poor, lower the value as appropriate to, for example, 30. This way, you can avoid the time-out of a large number of requests from request competition.
 |
    |workerMaxThroughput\(KB/s\)|The data migration traffic ceiling on the worker node.|This value limits the traffic. The default value 0 indicates that no traffic limitations are imposed.|
    |dispatcherThreadNum|The number of threads for task distribution and status confirmation of the tracker.|The default value must be enough. You don’t need to change the default value if you have no special requirements.|
    |workerAbortWhenUncatchedException|Whether to skip or cancel in case of an unknown error.|Unknown errors are skipped by default.|
    |workerRecordMd5|Whether to use metadata x-oss-meta-md5 to log the MD5 value of the migrated file in the OSS. The default setting is no.|It is mainly used for file data validation using MD5.|

-   job.cfg

    Data migration job configuration. The local\_job.cfg and job.cfg options are identical except in name.

    |Field|Meaning|Description|
    |:----|:------|:----------|
    |jobName|The job name, a string.|     -   The unique identifier of the job. The naming rule is \[a-zA-Z0-9\_-\]\{4,128\}. It supports the submission of multiple jobs of different names.
    -   If you submit a job with the same name as another job, the system prompts that the job already exists. You are not allowed to submit a job of the same name before you clean the original job with the name.
 |
    |jobType|The job type, a string.|There are two types: import and audit. The default value is import.    -   import: Run the data migration and validate the migrated data for consistency. 
    -   audit: Only validate data consistency.
|
    |isIncremental|Whether to enable incremental migration mode, a Boolean value.|     -   Default value: False.
    -   If it is set to true, incremental data is rescanned at the interval specified by incrementalModeInterval \(unit: second\) and synchronized to the OSS.
 |
    |incrementalModeInterval|Synchronization interval in incremental mode, an integer value. Unit: second.|Valid when isIncremental=true. The minimum configurable interval is 900 seconds. We do not recommend you configure it to a value smaller than 3,600 seconds as that wastes a large number of requests and lead to additional system overhead.|
    |importSince|Migrate data later than this time value, an integer value. Unit: second.|     -   This time value is a Unix timestamp, that is, the number of seconds since UTC 00:00 on January 1, 1970. You can get the value through the date +%s command.
    -   The default value is 0, indicating to migrate all the data.
 |
    |srcType|The synchronization source type, a string. Case sensitive.|Currently this parameter supports 10 types including local, oss, qiniu, bos, ks3, s3, youpai, HTTP, cos, and azure.     -   local: Migrate data from a local file to the OSS. You only need to enter the srcPrefix for this option and do not need to enter srcAccessKey, srcSecretKey, srcDomain, and srcBucket.
    -   Migrate data from one bucket to another. 
    -   qiniu: Migrate data from Qiniu cloud storage to the OSS.
    -   bos: Migrate data from Baidu cloud storage to the OSS. 
    -   ks3: Migrate data from Kingsoft cloud storage to the OSS. 
    -   s3: Migrate data from AWS S3 to the OSS. 
    -   youpai: Migrate data from Youpai Cloud to the OSS.
    -   HTTP: Migrate data to the OSS through the provided HTTP link list.
    -   cos: Migrate data from the Tencent cloud storage COS to the OSS.
    -   azure: Migrate data from Azure Blob to the OSS.
|
    |srcAccessKey|The source AccessKey, a string.|Enter the AccessKey of the data source if srcType is set to oss, qiniu, baidu, ks3, or s3.     -   or the local and HTTP types, this option can be left empty.
    -   For youpai and azure types, enter the AccountName.
|
    |srcSecretKey|The source SecretKey, a string.|Enter the SecretKey of the data source if srcType is set to oss, qiniu, baidu, ks3, or s3.    -   For the local and HTTP types, this option can be left empty.
    -   youpai: Enter the operator password.
    -   azure: Enter the AccountKey.
|
    |srcDomain|Source endpoint.|This configuration item is not required if the srcType is set to local or HTTP.     -   oss: The domain name obtained from the console. It is a second-level domain name without the bucket prefix. A full list can be found at domain name list.
    -   qiniu: The domain name of the corresponding bucket obtained from the Qiniu console.
    -   bos: The Baidu BOS domain name, such as `http://bj.bcebos.com` or `http://gz.bcebos.com`.
    -   ks3: Kingsoft KS3 domain name, such as `http://kss.ksyun.com`, `http://ks3-cn-beijing.ksyun.com` or `http://ks3-us-west-1.ksyun.coms`.
    -   The S3 and AWS S3 domain names of various regions can be found at S3 Endpoint.
    -   youpai: The domain name of the Youpai Cloud, such as automatic identification of the optimal channel of `http://v0.api.upyun.com`, or telecommunication line `http://v1.api.upyun.com`, or China Unicom or China Netcom line `http://v2.api.upyun.com`, or China Mobile or China Railcom line `http://v3.api.upyun.com`.
    -   cos: The bucket region of the Tencent Cloud, such as South China: gz, North China: tj, and East China: sh.
    -   azure: The EndpointSuffix in the Azure Blob connection string, such as core.chinacloudapi.cn.
|
    |srcBucket|The name of the source bucket or the container.|This configuration item is not required if the srcType is set to local or HTTP. azure: Enter the container name in Azure Blob, and enter the bucket name for others.|
    |srcPrefix|The source prefix, a string. The default value is empty.|If the srcType is set to local, enter the local directory in full, separated, and ended by /, such as c:/example/ or /data/example/. If the srcType is oss, qiniu, bos, ks3, youpai, or s3, the value is the prefix of the object to be synchronized, without the bucket name, such as data/to/oss/. If you want to synchronize all the objects, leave the srcPrefix empty.|
    |destAccessKey|The destination AccessKey, a string.|To view the OSS AccessKeyID, log on to the [console](https://oss.console.aliyun.com/index#/).

.|
    |destSecretKey|The destination SecretKey, a string.|To view the OSS AccessKeySecret, log on to the [console](https://oss.console.aliyun.com/index#/).

.|
    |destDomain|Destination endpoint, a string.|Obtained from the [console](https://oss.console.aliyun.com/index#/). It is a second-level domain name without the bucket prefix. A full list can be found at domain name list.

.|
    |destBucket|The destination bucket, a string.|The OSS bucket name. It does not need to end with /.|
    |destPrefix|The destination prefix, a string. The default value is empty.|     -   The destination prefix. The default value is empty in which case the objects are placed in the destination bucket.
    -   If you want to synchronize data to a specific directory on the OSS, end the prefix with /, such as data/in/oss/. 
    -   Note that the OSS does not support / as the object prefix, so do not set destPrefix to start with /.
    -   A local file in the path srcPrefix+relativePath is migrated to destDomain/destBucket/destPrefix + relativePath on the OSS.
    -   An object on the cloud in the path srcDomain/srcBucket/srcPrefix+relativePath is migrated to destDomain/destBucket/destPrefix + relativePath on the OSS.
 |
    |taskObjectCountLimit|The maximum number of files in a task, an integer. The default value is 10,000. |This configuration option affects the concurrency of the executed jobs. Generally the configuration is set to the total number of files/total number of workers/number of migration threads \(workerTaskThreadNum\) and the maximum number is 50,000. If the total number of files is unknown, use the default value.|
    |taskObjectSizeLimit|The maximum data size in a task, an integer. Unit: bytes. The default value is 1 GB.|This configuration option affects the concurrency of the executed jobs. Generally the configuration is set to the total data size/total number of workers/number of migration threads \(workerTaskThreadNum\). If the total data size is unknown, use the default value.|
    |isSkipExistFile|Whether to skip the existing objects during data migration, a Boolean value.|If it is set to true, the objects are skipped according to the size and LastModifiedTime. If it is set to false, the existing objects are overwritten. The default value is false. This option is invalid when jobType is set to audit.|
    |scanThreadCount|The number of threads for parallel file scanning, an integer. The default value is 1.|This configuration option is related to file scanning efficiency. Do not modify the configuration if you have no special requirements.|
    |maxMultiThreadScanDepth|The maximum allowable depth of directories for the parallel scan, an integer. The default value is 1.|     -   The default value of 1 indicates parallel scan on top-level directories.
    -   Do not modify this configuration if you have no special requirements. If the value is configured too large, the job may fail to run normally.
 |
    |appId|The appId of the Tencent COS, an integer.|Valid when srcType is set to cos.|
    |httpListFilePath|The absolute path of the HTTP list file, a string.|     -   Valid when srcType is set to HTTP. When the source is an HTTP link address, you are required to provide the absolute path of the file with the HTTP link address as the content, such as c:/example/http.list.
    -   The HTTP link in the file must be divided into two columns separated by spaces, representing the prefix and the relative path on the OSS after the upload respectively, such as c:/example/http.list which contains the following content: `http://mingdi-hz.oss-cn-hangzhou.aliyuncs.com/aa/ bb.jpg` `http://mingdi-hz.oss-cn-hangzhou.aliyuncs.com/cc/dd.jpg`. The object names for the two rows after they are migrated to the OSS are destPrefix + bb.jpg and destPrefix + cc/dd.jpg respectively.
 |

-   Workers

    The workers is exclusive to the distributed mode and every IP address is a row,  such as:

    ```
    192.168.1.6
    192.168.1.7
    192.168.1.8
    ```

    Note:

    -   In the preceding configuration, the `192.168.1.6` in the first line must be master, that is, the master, worker, and TaskTracker are started on`192.168.1.6` and the console also needs to be executed on the machine.
    -   Make sure that the user name, logon mode, and working directory of multiple worker modes are the same.

## Configuration file example {#section_yjs_3gh_wdb .section}

The data migration task profile for a distributed deployment is shown in the following table, and the configuration file name for a stand-alone machine is `local_job.cfg`, there is no difference between a configuration item and a distributed deployment.

|Migration type|Configuration File|Description:|
|:-------------|:-----------------|:-----------|
|Migrate locally to OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/local/job.cfg)|Srcprefix is an absolute path at the end of`/`, such `D:/work/oss/data/`, `/home/user/work/oss/data/`|
|Migrating from seven bull cloud storage to OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/qiniu/job.cfg)|Srcprefix and DESTIN prefix can be configured to be empty; if not empty, end with `/` such as `destPrefix=docs/`|
|Transfer from Baidu Bos to OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/bos/job.cfg)|Srcprefix and DESTIN prefix can be configured to be empty; if not empty, end with `/`, such as `destPrefix=docs/`|
|Migrating from AWS S3 to OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/s3/job.cfg)|[domain names](http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) for S3|
|Move from cloud storage to OSS again|[job.cfg](https://github.com/aliyun/ossimport/tree/master/conf/youpai)|Srcaccesskey/Scanner enters the operator account number and password|
|Migrating from Tencent COs to OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/cos/job.cfg)|Srcdomain please follow V4 version, such as `srcDomain=sh`. Srcprefix can be empty, when not empty, start and end with `/`, such as `srcPrefix=/docs/`|
|Migrating from azure blob to OSS|[job.cfg](https://github.com/aliyun/ossimport/tree/master/conf/blob)|Srcaccesskey/srcsecretkey fill storage cun chu and key; srcdomain enters connection string Endpointsuffix, such as `core.chinacloudapi.cn`.|
|Migrating from OSS to OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/oss/job.cfg)|It is suitable for data migration between different regions, between Different Storage types, and between different prefixes; it is recommended to deploy on ECS and use domain names with internal to save on traffic.|
|Migrate data to OSS using high-speed channels|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/et/job.cfg)|Applies to all data sources, if you have high-speed migration requirements, submit a job or contact OSS support|

