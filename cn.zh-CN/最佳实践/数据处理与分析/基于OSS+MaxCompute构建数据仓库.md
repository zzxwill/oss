# 基于OSS+MaxCompute构建数据仓库 {#concept_sv1_3cv_ydb .concept}

本文介绍如何基于OSS并使用MaxCompute构建PB数据仓库。通过MaxCompute对OSS上的海量数据进行分析，将您的大数据分析工作效率提升至分钟级，帮助您更高效、更低成本的挖掘海量数据价值。

## 背景信息 {#section_wx3_5gd_zdb .section}

-   对象存储OSS

    对象存储OSS提供标准、低频、归档存储类型，能够覆盖从热到冷的不同存储场景。同时，OSS能够与Hadoop开源社区及EMR、批量计算、MaxCompute、机器学习PAI、DatalakeAnalytics、函数计算等阿里云计算产品进行深度结合。

    用户可以打造基于OSS的数据分析应用，如MapReduce、HIVE/Pig/Spark等批处理（如日志离线计算）、交互式查询分析（Imapla、Presto、DataLakeAnalytics）、深度学习训练（阿里云PAI）、基因渲染计算交付（批量计算）、大数据应用（MaxCompute）及流式处理（函数计算）等。

-   MaxCompute

    MaxCompute是一项大数据计算服务，能够提供快速且完全托管的数据仓库解决方案，并可以与OSS结合，高效并经济地分析处理海量数据。MaxCompute的处理性能达到了全球领先水平，被Forrester评为全球云端数据仓库领导者。

-   OSS外部表查询功能

    MaxCompute重磅推出了一项重要特性：OSS外表查询功能。该功能可以帮助您直接对OSS中的海量文件进行查询，而不必将数据加载到MaxCompute表中，既节约了数据搬迁的时间和人力，也节省了多地存储的成本。

    使用MaxCompute+OSS的方案，您可以获得以下优势：

    -   MaxCompute是一个无服务器的分布式计算架构，无需用户对服务器基础设施进行额外的维护和管理，能够根据OSS用户的需求方便及时地提供临时查询服务，帮助企业大幅节省成本。
    -   OSS为海量的对象存储服务。用户将数据统一存储在OSS，可以做到计算和存储分离，多种计算应用和业务都可以访问使用OSS的数据，数据只需要存储一份。
    -   支持处理OSS上开源格式的结构化文件，包括：Avro、CSV、ORC、Parquet、RCFile、RegexSerDe、SequenceFile和TextFile，同时支持gzip压缩格式。

## 应用场景 {#section_wgn_chd_zdb .section}

互联网金融应用每天都需要将大量的金融数据交换文件存放在OSS上，并需要进行超大文本文件的结构化分析。通过MaxCompute的OSS外部表查询功能，用户可以直接用外部表的方式将OSS上的大文件加载到MaxCompute进行分析，从而大幅提升整个链路的效率。

## 操作步骤 {#section_edy_dhd_zdb .section}

下面我们通过两个简单的示例，介绍如何通过MaxCompute外表功能实现对OSS数据的分析和处理。

-   场景一：物联网采集数据分析
    1.  步骤1：准备工作
        1.  开通OSS和MaxCompute服务。

            您可以通过官网分别开通OSS、MaxCompute服务，并创建OSS bucket、MaxCompute Project。

        2.  采集数据到OSS。

            您可以使用任何数据集来执行测试，以验证我们在这篇文章中概述的最佳实践。本示例在OSS上准备了一批 CSV 数据，endpoint为oss-cn-beijing-internal.aliyuncs.com，bucket为oss-odps-test，数据文件的存放路径为 **/demo/vehicle.csv**。

        3.  授权MaxCompute访问OSS。

            MaxCompute需要直接访问OSS的数据，因此需要将OSS的数据相关权限赋给MaxCompute的访问账号。您可以在直接登录阿里云账号后，[点击完成授权](https://ram.console.aliyun.com/)。

    2.  步骤2：通过MaxCompute创建外部表

        创建外部表，语句如下：

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

    3.  步骤3：通过MaxCompute查询外部表

        成功创建外部表后，便可如普通表一样使用该外部表。

        假设 **/demo/vehicle.csv** 的数据如下：

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

        执行如下 SQL 语句：

        ```
        select recordId, patientId, direction from ambulance_data_csv_external where patientId > 25;
        ```

        输出结果如下：

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

        关于更多详细的OSS外表使用方法，请参考官方文档，[点击这里](https://help.aliyun.com/document_detail/54518.html)。

-   场景二：阿里云产品消费账单分析
    1.  步骤1：准备工作
        1.  开通OSS和MaxCompute服务。

            您可以通过官网分别开通OSS和MaxCompute服务，并创建OSS bucket、MaxCompute Project。

        2.  授权MaxCompute访问OSS。

            MaxCompute需要直接访问OSS的数据，因此需要将OSS的数据相关权限赋给MaxCompute的访问账号。您可以在直接登录阿里云账号后，[点击完成授权](https://ram.console.aliyun.com/)。

    2.  步骤2：通过费用中心同步账单数据到OSS
        1.  在阿里云控制台中，点击**费用中心** \> **消费记录** \> **存储到OSS**，输入用于存储文件的OSS bucket名称，本示例中为：**oms-yl**，如下图所示：

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13965/15498700584643_zh-CN.jpg)

        2.  服务开通后，每天会将增量的实例消费明细数据生成文件，并同步存储到指定的bucket中，如下图所示：

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13965/15498700584644_zh-CN.jpg)

    3.  步骤3：通过MaxCompute注册账单处理类
        1.  点击以下链接下载自定义代码：[odps-udf-example-0.30.0-SNAPSHOT-jar-with-dependencies.jar](https://yq.aliyun.com/attachment/download/?spm=a2c4e.11153940.blogcont591731.24.7f105f22jikN14&id=5495)
        2.  运行以下命令，将自定义代码编译打包，并上传到 MaxCompute。

            ```
            add jar odps-udf-example-0.30.0-SNAPSHOT-jar-with-dependencies.jar
            ```

    4.  步骤4：通过MaxCompute创建外部表

        以创建5月4日的账单消费表为例，外部表如下：

        ```
        CREATE EXTERNAL TABLE IF NOT EXISTS oms_oss_0504
        (
        月份 string,
        资源拥有者 string,
        消费时间 string,
        消费类型 string,
        账单编号 string,
        商品 string,
        计费方式 string,
        服务开始时间 string,
        服务结束时间 string,
        服务时长 string,
        财务核算单元 string,
        资源id string,
        资源昵称 string,
        TAG string,
        地域 string,
        可用区 string,
        公网ip string,
        内网ip string,
        资源配置 string,
        原价 string,
        优惠金额 string,
        应付金额 string,
        计费项1 string,
        使用量1 string,
        资源包扣除1 string,
        原价1 string ,
        应付金额1 string,
        计费项2 string,
        使用量2 string,
        资源包扣除2 string,
        原价2 string,
        应付金额2 string,
        计费项3 string,
        使用量3 string,
        资源包扣除3 string,
        原价3 string,
        应付金额3 string,
        计费项4 string,
        使用量4 string,
        资源包扣除4 string,
        原价4 string,
        应付金额4 string,
        计费项5 string,
        使用量5 string,
        资源包扣除5 string,
        原价5 string,
        应付金额5 string,
        计费项6 string,
        使用量6 string,
        资源包扣除6 string,
        原价6 string,
        应付金额6 string,
        计费项7 string,
        使用量7 string,
        资源包扣除7 string,
        原价7 string,
        应付金额7 string,
        计费项8 string,
        使用量8 string,
        资源包扣除8 string,
        原价8 string,
        应付金额8 string,
        计费项9 string,
        使用量9 string,
        资源包扣除9 string,
        原价9 string,
        应付金额9 string
        )
        STORED BY 'com.aliyun.odps.udf.example.text.TextStorageHandler' --STORED BY 指定自定义 StorageHandler 的类名。
        with SERDEPROPERTIES (
        'odps.text.option.complex.text.enabled'='true', 
        'odps.text.option.strict.mode'='false'
        --遇到列数不一致的情况不会抛异常，如果实际列数少于schema列数，将所有列按顺序匹配，剩下的不足的列补NULL。
        )
        LOCATION 'oss://oss-cn-beijing-internal.aliyuncs.com/oms-yl/2018-05-04/'
        USING 'text_oss.jar'; --同时需要指定账单中的文本处理类定义所在的 jar 包。
        ```

    5.  步骤5：通过MaxCompute查询外部表

        以查询ECS快照消费账单明细为例，命令如下：

        ```
        select  月份,原价,优惠金额,应付金额,计费项1,使用量 from oms_oss where  商品=快照(SNAPSHOT);
        ```


