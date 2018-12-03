# Apache Impala（CDH6）查询OSS数据 {#concept_igt_g1w_wfb .concept}

CDH（Cloudera's Distribution, including Apache Hadoop）是众多 Hadoop 发行版本中的一种。最新版本 CDH6.0.1 中的 Hadoop 3.0.0 版本已经支持 OSS。本文介绍如何使 CDH6 的相关组件（Hadoop、Hive、Spark、Impala 等）能够查询 OSS。

## 准备工作 {#section_pwg_1gx_wfb .section}

您需要拥有一个已搭建好的 CDH6 集群。若没有已搭建好的 CDH6 集群，您需要参考官方文档，先搭建一个 CDH6 集群。

## 步骤一：增加 OSS 配置 {#section_gvt_2vx_wfb .section}

1.  通过集群管理工具 CM 来增加配置。若没有 CM 管理的集群，可以修改core-site.xml。以CM为例，需要增加如下配置：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64876/154380672133147_zh-CN.png)

    |配置项|值|
    |:--|:-|
    |**fs.oss.endpoint**|填写需要连接的 OSS 的 Endpoint。例如：oss-cn-zhangjiakou-internal.aliyuncs.com

|
    |**fs.oss.accessKeyId**|填写OSS的 AccessKeyId。|
    |**fs.oss.accessKeySecret**|填写OSS的 AccessKeySecret。|
    |**fs.oss.impl**|Hadoop OSS文件系统实现类。目前固定为：org.apache.hadoop.fs.aliyun.oss.AliyunOSSFileSystem|
    |**fs.oss.buffer.dir**|填写临时文件目录。建议值：/tmp/oss

|
    |**fs.oss.connection.secure.enabled**|是否开启HTTPS。开启HTTPS会影响性能。建议值：false

|
    |**fs.oss.connection.maximum**|与 OSS 的连接数建议值：2048

|

    更多参数解释可参考 [Hadoop-Aliyun module](https://github.com/apache/hadoop/blob/trunk/hadoop-tools/hadoop-aliyun/src/site/markdown/tools/hadoop-aliyun/index.md)。

2.  根据 CM 提示重启集群。
3.  测试读写 OSS：
    -   测试读：

        ```
        hadoop fs -ls oss://${your-bucket-name}/
        ```

    -   测试写：

        ```
        hadoop fs -mkdir oss://${your-bucket-name}/hadoop-test
        ```

        若测试可以读写 OSS，则配置成功；若无法读写 OSS，请检查配置。

        **说明：** 本文中所有 $\{\} 的内容为环境变量，请根据您实际的环境修改。


## 步骤二：配置 Apache Impala 对 OSS 的支持 {#section_fqy_tdb_yfb .section}

虽然 CDH6 支持 OSS，但是它里面的 Impala 组件却没有将 OSS 的支持放到它的 CLASSPATH 里面去，因此您需要在所有的 Impala 节点执行如下操作：

1.  进入$\{CDH\_HOME\}/lib/impala目录，执行如下命令：

    ```
    [root@cdh-master impala]# cd lib/
    [root@cdh-master lib]# ln -s ../../../jars/hadoop-aliyun-3.0.0-cdh6.0.1.jar hadoop-aliyun.jar
    [root@cdh-master lib]# ln -s ../../../jars/aliyun-sdk-oss-2.8.3.jar aliyun-sdk-oss-2.8.3.jar
    [root@cdh-master lib]# ln -s ../../../jars/jdom-1.1.jar jdom-1.1.jar
    ```

2.  进入$\{CDH\_HOME\}/bin目录，修改impalad、statestored、catalogd这三个文件，在文件最后一行exec命令前增加如下内容：

    ```
    export CLASSPATH=${CLASSPATH}:${IMPALA_HOME}/lib/hadoop-aliyun.jar:${IMPALA_HOME}/lib/aliyun-sdk-oss-2.8.3.jar:${IMPALA_HOME}/lib/jdom-1.1.jar
    ```

3.  重启所有节点的 impala 相关进程。

## 验证配置 {#section_nvt_2vx_wfb .section}

尝试使用 Apache Impala 查询 OSS，并且运行 TPC-DS 的查询语句。相关介绍请参考[Apache Impala 的介绍](http://impala.apache.org/)和[TPC-DS查询](https://github.com/hortonworks/hive-testbench/tree/hive14)。

TPC-DS 基于 Hive QL，因为 Hive QL与 Impala SQL兼容性比较高，所以我们用这个作为实验案例（有几个 query 因为兼容性问题不能运行）：

1.  生成 sample 数据到 OSS：

    ```
    [root@cdh-master ~]# git clone https://github.com/hortonworks/hive-testbench.git
    [root@cdh-master ~]# cd hive-testbench
    [root@cdh-master hive-testbench]# git checkout hive14
    [root@cdh-master hive-testbench]# ./tpcds-build.sh
    [root@cdh-master hive-testbench]# FORMAT=textfile ./tpcds-setup.sh 50 oss://{your-bucket-name}/
    ```

2.  建表。这个 benchmark 的建表语句与 Apache Impala 的建表语句兼容，复制ddl-tpcds/text/alltables.sql中的建表语句，修改$\{LOCATION\}即可，例如：

    ```
    [root@cdh-master hive-testbench]# impala-shell -i cdh-slave01 -d default
    Starting Impala Shell without Kerberos authentication
    Connected to cdh-slave01:21000
    Server version: impalad version 3.0.0-cdh6.0.1 RELEASE(build9a74a5053de5f7b8dd983802e6d75e58d31472db)
    ***********************************************************************************
    Welcome to the Impala shell.
    (Impala Shell v3.0.0-cdh6.0.1(9a74a50) built on Wed Sep 1911:27:37 PDT 2018)
    
    Want to know what versionof Impala you're connected to? Run the VERSION command to
    find out!
    ***********************************************************************************
    Query: use `default`
    [cdh-slave01:21000] default> create external table call_center(
                               >       cc_call_center_sk         bigint
                               > ,     cc_call_center_id         string
                               > ,     cc_rec_start_date        string
                               > ,     cc_rec_end_date          string
                               > ,     cc_closed_date_sk         bigint
                               > ,     cc_open_date_sk           bigint
                               > ,     cc_name                   string
                               > ,     cc_class                  string
                               > ,     cc_employees              int
                               > ,     cc_sq_ft                  int
                               > ,     cc_hours                  string
                               > ,     cc_manager                string
                               > ,     cc_mkt_id                 int
                               > ,     cc_mkt_class              string
                               > ,     cc_mkt_desc               string
                               > ,     cc_market_manager         string
                               > ,     cc_division               int
                               > ,     cc_division_name          string
                               > ,     cc_company                int
                               > ,     cc_company_name           string
                               > ,     cc_street_number          string
                               > ,     cc_street_name            string
                               > ,     cc_street_type            string
                               > ,     cc_suite_number           string
                               > ,     cc_city                   string
                               > ,     cc_county                 string
                               > ,     cc_state                  string
                               > ,     cc_zip                    string
                               > ,     cc_country                string
                               > ,     cc_gmt_offset             double
                               > ,     cc_tax_percentage         double
                               > )
                               > row format delimited fields terminated by '|'
                               > location 'oss://{your-bucket-name}/50/call_center';
    Query: create external table call_center(
          cc_call_center_sk         bigint
    ,     cc_call_center_id         string
    ,     cc_rec_start_date        string
    ,     cc_rec_end_date          string
    ,     cc_closed_date_sk         bigint
    ,     cc_open_date_sk           bigint
    ,     cc_name                   string
    ,     cc_class                  string
    ,     cc_employees              int
    ,     cc_sq_ft                  int
    ,     cc_hours                  string
    ,     cc_manager                string
    ,     cc_mkt_id                 int
    ,     cc_mkt_class              string
    ,     cc_mkt_desc               string
    ,     cc_market_manager         string
    ,     cc_division               int
    ,     cc_division_name          string
    ,     cc_company                int
    ,     cc_company_name           string
    ,     cc_street_number          string
    ,     cc_street_name            string
    ,     cc_street_type            string
    ,     cc_suite_number           string
    ,     cc_city                   string
    ,     cc_county                 string
    ,     cc_state                  string
    ,     cc_zip                    string
    ,     cc_country                string
    ,     cc_gmt_offset             double
    ,     cc_tax_percentage         double
    )
    row format delimited fields terminated by '|'
    location 'oss://{your-bucket-name}/50/call_center'
    +-------------------------+
    | summary                 |
    +-------------------------+
    | Table has been created. |
    +-------------------------+
    Fetched 1 row(s) in 4.10s
    ```

3.  检查建表内容：

    ```
    [cdh-slave01:21000] default> showtables;
    Query: showtables
    +------------------------+
    | name                   |
    +------------------------+
    | call_center            |
    | catalog_page           |
    | catalog_returns        |
    | catalog_sales          |
    | customer               |
    | customer_address       |
    | customer_demographics  |
    | date_dim               |
    | household_demographics |
    | income_band            |
    | inventory              |
    | item                   |
    | promotion              |
    | reason                 |
    | ship_mode              |
    | store                  |
    | store_returns          |
    | store_sales            |
    | time_dim               |
    | warehouse              |
    | web_page               |
    | web_returns            |
    | web_sales              |
    | web_site               |
    +------------------------+
    Fetched 24 row(s) in 0.03s
    ```

4.  在 Impala 上执行 TPC-DS 的查询，已经可以正常查询 OSS 的数据。

    1.  查询的 SQL 文件在sample-queries-tpcds目录下：

        ```
        [root@cdh-master hive-testbench]# ls sample-queries-tpcds
        query12.sql  query20.sql  query27.sql  query39.sql  query46.sql  query54.sql  query64.sql  query71.sql  query7.sql   query87.sql  query93.sql  README.md
        query13.sql  query21.sql  query28.sql  query3.sql   query48.sql  query55.sql  query65.sql  query72.sql  query80.sql  query88.sql  query94.sql  testbench.settings
        query15.sql  query22.sql  query29.sql  query40.sql  query49.sql  query56.sql  query66.sql  query73.sql  query82.sql  query89.sql  query95.sql  testbench-withATS.settings
        query17.sql  query24.sql  query31.sql  query42.sql  query50.sql  query58.sql  query67.sql  query75.sql  query83.sql  query90.sql  query96.sql
        query18.sql  query25.sql  query32.sql  query43.sql  query51.sql  query60.sql  query68.sql  query76.sql  query84.sql  query91.sql  query97.sql
        query19.sql  query26.sql  query34.sql  query45.sql  query52.sql  query63.sql  query70.sql  query79.sql  query85.sql  query92.sql  query98.sql
        ```

    2.  执行 TPC-DS 查询query13.sql：

        ```
        [root@cdh-master hive-testbench]# impala-shell -i cdh-slave01 -d default -f sample-queries-tpcds/query13.sql
        Starting Impala Shell without Kerberos authentication
        Connected to cdh-slave01:21000
        Server version: impalad version 3.0.0-cdh6.0.1 RELEASE(build9a74a5053de5f7b8dd983802e6d75e58d31472db)
        Query: use`default`Query: selectavg(ss_quantity)
               ,avg(ss_ext_sales_price)
               ,avg(ss_ext_wholesale_cost)
               ,sum(ss_ext_wholesale_cost)
         from store_sales
             ,store
             ,customer_demographics
             ,household_demographics
             ,customer_address
             ,date_dim
         where store.s_store_sk = store_sales.ss_store_sk
         and  store_sales.ss_sold_date_sk = date_dim.d_date_sk and date_dim.d_year = 2001and((store_sales.ss_hdemo_sk=household_demographics.hd_demo_sk
          and customer_demographics.cd_demo_sk = store_sales.ss_cdemo_sk
          and customer_demographics.cd_marital_status = 'M'and customer_demographics.cd_education_status = '4 yr Degree'and store_sales.ss_sales_price between100.00and150.00and household_demographics.hd_dep_count = 3
             )or
            (store_sales.ss_hdemo_sk=household_demographics.hd_demo_sk
          and customer_demographics.cd_demo_sk = store_sales.ss_cdemo_sk
          and customer_demographics.cd_marital_status = 'D'and customer_demographics.cd_education_status = 'Primary'and store_sales.ss_sales_price between50.00and100.00and household_demographics.hd_dep_count = 1
             ) or
            (store_sales.ss_hdemo_sk=household_demographics.hd_demo_sk
          and customer_demographics.cd_demo_sk = ss_cdemo_sk
          and customer_demographics.cd_marital_status = 'U'and customer_demographics.cd_education_status = 'Advanced Degree'and store_sales.ss_sales_price between150.00and200.00and household_demographics.hd_dep_count = 1
             ))
         and((store_sales.ss_addr_sk = customer_address.ca_address_sk
          and customer_address.ca_country = 'United States'and customer_address.ca_state in('KY', 'GA', 'NM')
          and store_sales.ss_net_profit between100and200
             ) or
            (store_sales.ss_addr_sk = customer_address.ca_address_sk
          and customer_address.ca_country = 'United States'and customer_address.ca_state in('MT', 'OR', 'IN')
          and store_sales.ss_net_profit between150and300
             ) or
            (store_sales.ss_addr_sk = customer_address.ca_address_sk
          and customer_address.ca_country = 'United States'and customer_address.ca_state in('WI', 'MO', 'WV')
          and store_sales.ss_net_profit between50and250
             ))
        Query submitted at: 2018-10-3011:44:47(Coordinator: http://cdh-slave01:25000)
        Query progress can be monitored at: http://cdh-slave01:25000/query_plan?query_id=ff4b3157eddfc3c4:8a31c6500000000
        +-------------------+-------------------------+----------------------------+----------------------------+
        | avg(ss_quantity)  | avg(ss_ext_sales_price) | avg(ss_ext_wholesale_cost) | sum(ss_ext_wholesale_cost) |
        +-------------------+-------------------------+----------------------------+----------------------------+
        | 30.87106918238994 | 2352.642327044025       | 2162.600911949685          | 687707.09                  |
        +-------------------+-------------------------+----------------------------+----------------------------+
        Fetched 1 row(s) in 353.16s
        ```

    本次查询涉及到6张表，store、store\_sales、customer\_demographics、household\_demographics、customer\_address、date\_dim：

    ```
    [cdh-slave01:21000] default> showtable STATS store;
    Query: showtable STATS store
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------+
    | #Rows | #Files | Size    | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                  |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------+
    | -1    | 1      | 37.56KB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/store |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------+
    Fetched 1 row(s) in 0.01s
    [cdh-slave01:21000] default> showtable STATS store_sales;
    Query: showtable STATS store_sales
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------------+
    | #Rows | #Files | Size    | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                        |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------------+
    | -1    | 50     | 18.75GB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/store_sales |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------------+
    Fetched 1 row(s) in 0.01s
    [cdh-slave01:21000] default> showtable STATS customer_demographics;
    Query: showtable STATS customer_demographics
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-----------------------------------------------------------+
    | #Rows | #Files | Size    | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                                  |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-----------------------------------------------------------+
    | -1    | 50     | 76.92MB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/customer_demographics |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-----------------------------------------------------------+
    Fetched 1 row(s) in 0.01s
    [cdh-slave01:21000] default> showtable STATS household_demographics;
    Query: showtable STATS household_demographics
    +-------+--------+----------+--------------+-------------------+--------+-------------------+------------------------------------------------------------+
    | #Rows | #Files | Size     | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                                   |
    +-------+--------+----------+--------------+-------------------+--------+-------------------+------------------------------------------------------------+
    | -1    | 1      | 148.10KB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/household_demographics |
    +-------+--------+----------+--------------+-------------------+--------+-------------------+------------------------------------------------------------+
    Fetched 1 row(s) in 0.01s
    [cdh-slave01:21000] default> showtable STATS customer_address;
    Query: showtable STATS customer_address
    +-------+--------+---------+--------------+-------------------+--------+-------------------+------------------------------------------------------+
    | #Rows | #Files | Size    | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                             |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+------------------------------------------------------+
    | -1    | 1      | 40.54MB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/customer_address |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+------------------------------------------------------+
    Fetched 1 row(s) in 0.01s
    [cdh-slave01:21000] default> showtable STATS date_dim;
    Query: showtable STATS date_dim
    +-------+--------+--------+--------------+-------------------+--------+-------------------+----------------------------------------------+
    | #Rows | #Files | Size   | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                     |
    +-------+--------+--------+--------------+-------------------+--------+-------------------+----------------------------------------------+
    | -1    | 1      | 9.84MB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/date_dim |
    +-------+--------+--------+--------------+-------------------+--------+-------------------+----------------------------------------------+
    Fetched 1 row(s) in 0.01s
    ```


## 参考文档 {#section_r3x_mcr_wfb .section}

更多内容，可参考以下文档：

-   [Hadoop 支持集成 OSS](https://yq.aliyun.com/articles/292792?spm=a2c4e.11155435.0.0.7ccba82fbDwfhK)
-   [Hadoop](https://github.com/apache/hadoop/blob/trunk/hadoop-tools/hadoop-aliyun/src/site/markdown/tools/hadoop-aliyun/index.md)
-   [CDH5 Hadoop如何读写OSS](cn.zh-CN/最佳实践/数据处理与分析/CDH5 Hadoop如何读写OSS.md#)
-   [HDP2.6 Hadoop如何读写OSS](cn.zh-CN/最佳实践/数据处理与分析/HDP2.6 Hadoop如何读写OSS.md#)

您也可以通过阿里云 EMR 访问 OSS。阿里云 EMR 基于开源生态，包括 Hadoop、Spark、Kafka、Flink、Storm 等组件，为您提供集群、作业、数据管理等服务的一站式企业大数据平台，并无缝支持 OSS。阿里云 EMR 与 OSS 紧密结合，针对开源生态访问 OSS，有多项技术优化，详情可参考 [EMR产品介绍](https://www.aliyun.com/product/emapreduce))。

