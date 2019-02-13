# Construct a data warehouse by using OSS and MaxCompute {#concept_sv1_3cv_ydb .concept}

This topic describes the method of using MaxCompute to construct a PB data warehouse based on OSS. By using MaxCompute to analyze the massive data stored in OSS, you can complete your big data analysis tasks in minutes and explore data value more efficiently.

## Features {#section_wx3_5gd_zdb .section}

-   Object Storage Service \(OSS\)

    OSS provides three storage classes: Standard, Infrequent Access, and Archive, which are suitable for different data access scenarios. OSS can be used together with Hadoop open-source community products and multiple Alibaba Cloud products, such as EMR, BatchCompute, MaxCompute, machine learning tool PAI, Data Lake Analytics, and Function Compute.

    You can create several data analysis applications by using OSS, including:

    -   Batch processing applications using MapReduce, HIVE, Pig, or Spark, such as offline log computing
    -   Interactive query analysis applications, such as Imapla, Presto, and Data Lake Analytics
    -   Deep machine training applications, such as Alibaba Cloud PAI
    -   Gene rendering computing and delivery applications, such as BatchCompute
    -   Big data applications, such as MaxCompute
    -   Flow processing applications, such as Function Compute
-   MaxCompute

    MaxCompute is a big data computing service that provides fast and fully managed data warehouse solutions. MaxCompute can be used together with OSS to analyze and process massive data efficiently. With world-leading processing performance, MaxCompute was rated as the world's leading cloud-based data warehouse by Forrester.

-   OSS-external table query

    One major feature of MaxCompute is the OSS-external table query function, which can help you query massive objects stored in OSS directly without needing to load data into MaxCompute tables. This can help you move data more efficiently and eliminates the need to store data in multiple places.


A data warehouse solution constructed by using MaxCompute and OSS has the following advantages:

-   MaxCompute operates on a serverless, distributed computing architecture. Therefore, it allows for temporary query services in a timely manner based on the requirements of OSS users and does not require additional maintenance or management for server infrastructures, significantly reducing enterprise costs.
-   OSS provides storage for massive data that can be accessed by multiple computing applications and services. As a result, you can effectively separate computing and storage resources by storing only one copy of data in OSS.
-   A data warehouse solution using OSS and MaxCompute allows you to easily process structured files in open-source formats. Currently, the open-source formats supported are Avro, CSV, ORC, Parquet, RCFile, RegexSerDe, SequenceFile, and TextFile. The gzip compression format is also supported.

## Application scenario {#section_wgn_chd_zdb .section}

Financial applications on the Internet need to store a large number of financial data exchange files on OSS every day and must perform structured analysis on large test files. By using the OSS-external table query feature of MaxCompute, these requirements can be met with greater ease. Large files can be loaded on OSS to MaxCompute by using external tables. This process can significantly improve overall efficiency.

## Example: Analyze data collected from Internet of Things \(IoT\) {#section_qzx_kgn_tgb .section}

1.  Activate OSS. For more information, see [Sign up for OSS](../../../../../reseller.en-US/Quick Start/Sign up for OSS.md#).
2.  Create a bucket. For more information, see [创建Bucket](../../../../../reseller.en-US/Console User Guide/Manage buckets/Create a bucket.md#).
3.  Activate MaxCompute. For more information, see [Activate MaxCompute](../../../../../reseller.en-US/Prepare/Activate MaxCompute.md#).
4.  Create a MaxCompute project. For more information, see [创建MaxCompute Project](../../../../../reseller.en-US/Prepare/Create a project.md#).
5.  Grant OSS access permissions to MaxCompute.

    You must grant OSS access permissions to the account you used to access MaxCompute because MaxCompute needs to directly access data in OSS. You can log on to the RAM console with your Alibaba Cloud account to [grant permissions](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.16.761b1cdfvC1ITJ#role/authorize?request=%7B%22Requests%22:%20%7B%22request1%22:%20%7B%22RoleName%22:%20%22AliyunODPSDefaultRole%22,%20%22TemplateId%22:%20%22DefaultRole%22%7D%7D,%20%22ReturnUrl%22:%20%22https:%2F%2Fram.console.aliyun.com%2F%22,%20%22Service%22:%20%22ODPS%22%7D).

6.  Upload the data collected from IoT to OSS.

    **Note:** You can use any data set to test the procedures described in this topic. In this example, a CSV file named vehicle.csv has been uploaded to the /demo directory in an OSS bucket named oss-odps-test. The endpoint of the bucket is oss-cn-beijing-internal.aliyuncs.com.

7.  Run the following commands to create an external table in MaxCompute. For more information, see [创建表](../../../../../reseller.en-US/Quick Start/Create and view a table.md#).

    ```
    CREATE EXTERNAL TABLE IF NOT EXISTS ambulance_data_csv_external
    (
        vehicleId int,
        recordId int,
        patientId int,
        calls int,
        locationLatitute double,
        locationLongtitue double,
        recordTime string,
        direction string
        )
        STORED BY 'com.aliyun.odps.CsvStorageHandler'
        LOCATION 'oss://oss-cn-beijing-internal.aliyuncs.com/oss-odps-test/Demo/';
    ```

8.  Use MaxCompute to query the external table. You can use an external table in the same way as you use a normal table. For more information, see [../../../../../dita-oss-bucket/SP\_76/DNODPS1883056/EN-US\_TP\_11952.md\#section\_ynz\_3mq\_kgb](../../../../../reseller.en-US/Quick Start/Run SQL.md#section_ynz_3mq_kgb).

    Assume that the /demo/vehicle.csv file includes the following data:

    ```
    1,1,51,1,46.81006,-92.08174,9/14/2014 0:00,S
    1,2,13,1,46.81006,-92.08174,9/14/2014 0:00,NE
    1,3,48,1,46.81006,-92.08174,9/14/2014 0:00,NE
    1,4,30,1,46.81006,-92.08174,9/14/2014 0:00,W
    1,5,47,1,46.81006,-92.08174,9/14/2014 0:00,S
    1,6,9,1,46.81006,-92.08174,9/14/2014 0:00,S
    1,7,53,1,46.81006,-92.08174,9/14/2014 0:00,N
    1,8,63,1,46.81006,-92.08174,9/14/2014 0:00,SW
    1,9,4,1,46.81006,-92.08174,9/14/2014 0:00,NE
    1,10,31,1,46.81006,-92.08174,9/14/2014 0:00,N
    ```

    Run the following SQL statement:

    ```
    select recordId, patientId, direction from ambulance_data_csv_external where patientId > 25;
    ```

    The following results are returned.

    ```
    +------------+------------+-----------+
    | recordId   | patientId  | direction |
    +------------+------------+-----------+
    | 1          | 51         | S         |
    | 3          | 48         | NE        |
    | 4          | 30         | W         |
    | 5          | 47         | S         |
    | 7          | 53         | N         |
    | 8          | 63         | SW        |
    | 10         | 31         | N         |
    +------------+------------+-----------+
    ```

    For more information about the usage of OSS-external tables, see [Overview of External tables](../../../../../reseller.en-US/User Guide/External table/Overview of External tables.md#).


