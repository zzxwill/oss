# DataLakeAnalytics+OSS：基于OSS的Severless的交互式查询分析 {#concept_l1p_yls_c2b .concept}

## 背景 {#section_asr_yms_c2b .section}

本文以基金交易数据处理为例，介绍将数据存储在OSS，使用DataLakeAnalytics进行Severless的交互式查询分析。

## 对象存储OSS {#section_wwg_dns_c2b .section}

对象存储OSS提供标准、低频、归档存储类型，能够覆盖从热到冷的不同存储场景。同时，OSS能够与Hadoop开源社区及EMR、批量计算、MaxCompute、机器学习PAI、DatalakeAnalytics、函数计算等阿里云计算产品进行深度结合。

用户可以打造基于OSS的数据分析应用，如MapReduce、HIVE/Pig/Spark等批处理（如日志离线计算）、交互式查询分析（Imapla、Presto、DataLakeAnalytics）、深度学习训练（阿里云PAI）、基因渲染计算交付（批量计算）、大数据应用（MaxCompute）及流式处理（函数计算）等。

## DataLakeAnalytics {#section_rv2_2ns_c2b .section}

Data Lake Analytics是无服务器（Serverless）化的云上交互式查询分析服务。无需ETL，就可通过此服务在云上通过标准JDBC直接对阿里云OSS、TableStore的数据轻松进行查询和分析，以及无缝集成商业分析工具。

## 服务开通 {#section_sng_fns_c2b .section}

-   OSS服务：

    [开通OSS服务](https://www.aliyun.com/product/oss)

-   DataLakeAnalytics服务：

    [开通DataLakeAnalytics服务](https://www.aliyun.com/product/datalakeanalytics)


## 数据导出到OSS {#section_a24_pns_c2b .section}

以基金交易数据为例：

假设在OSS上存储了以下trade\\user文件夹，并存储相应的交易数据和开户信息：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14737/6242_zh-CN.png)

[下载模拟数据](http://testdatasample.oss-cn-hangzhou.aliyuncs.com/workshop_sh/workshop_sh.zip?spm=a2c4g.11186623.2.6.krrnZY&file=workshop_sh.zip)（该数据为以下实验中使用的模拟数据。）

## 登录Data Lake Analytics控制台 {#section_eqy_mqs_c2b .section}

点击[登录数据库](https://openanalytics.console.aliyun.com/)，使用方法可以参考 DataLakeAnalytics 产品帮助文档。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14737/6243_zh-CN.png)

## 创建Schema和Table {#section_uwg_4rs_c2b .section}

-   创建Schema

    输入创建SCHEMA的语句，点击**同步执行**。

    ```
    CREATE SCHEMA sh_trade
    ```

    CREATE SCHEMA sh\_trade（注意：同一个阿里云region，schema名全局唯一，建议根据业务定义。如有重名schema，在创建时会提示报错，则需换另一个schema名。）

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14737/6245_zh-CN.png)

-   创建Table

    在数据库的下拉框中，选择刚刚创建的schema。

    **说明：** 创建交易记录表及创建开户信息表时：

    -   LOCATION ‘oss://Bucket名称/交易记录表目录/‘
    -   Location替换为您的Bucket和测试数据的路径
    -   创建交易记录表：

        在SQL文本框中输入建表语句如下：

        ```
        CREATE EXTERNAL TABLE tradelist_csv (
            t_userid STRING COMMENT '用户ID',
            t_dealdate STRING COMMENT '申请时间', 
            t_businflag STRING COMMENT '业务代码', 
            t_cdate STRING COMMENT '确认日期', 
            t_date STRING COMMENT '申请日期',
            t_serialno STRING COMMENT'申请序号', 
            t_agencyno STRING COMMENT'销售商编号', 
            t_netno STRING  COMMENT'网点编号',
            t_fundacco STRING COMMENT'基金账号',
            t_tradeacco STRING COMMENT'交易账号',
            t_fundcode STRING  COMMENT'基金代码',
            t_sharetype STRING COMMENT'份额类别',
            t_confirmbalance DOUBLE  COMMENT'确认金额',
            t_tradefare DOUBLE COMMENT'交易费',
            t_backfare DOUBLE COMMENT'后收手续费',
            t_otherfare1 DOUBLE COMMENT'其他费用1',
            t_remark STRING COMMENT'备注'
            )
            ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
            STORED AS TEXTFIlE
            LOCATION 'oss://testdatasample/workshop_sh/trade/';
        ```

    -   创建开户信息表：

        ```
        CREATE EXTERNAL TABLE userinfo (
            u_userid STRING COMMENT '用户ID',
            u_accountdate STRING COMMENT '开户时间', 
            u_gender STRING COMMENT '性别', 
            u_age INT COMMENT '年龄', 
            u_risk_tolerance INT COMMENT '风险承受能力，1-10，10为最高级',
            u_city STRING COMMENT'所在城市', 
            u_job STRING COMMENT'工作类别， A-K', 
            u_income DOUBLE  COMMENT'年收入(万)'
            )
            ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
            STORED AS TEXTFIlE
            LOCATION 'oss://testdatasample/workshop_sh/user/';
        ```

        建表完毕后，刷新页面，在左边导航条中能看到schema下的2张表。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14737/6246_zh-CN.png)


## SQL查询\(同步执行\) {#section_pxm_xws_c2b .section}

-   查询交易机构SXS\_0010，在0603至0604的100条交易记录
    1.  查询SQL

        ```
        SELECT * FROM tradelist_csv 
        WHERE t_cdate >= '2018-06-03' and t_cdate <= '2018-06-04' and t_agencyno = 'SXS_0010' 
        limit 100;
        ```

    2.  显示执行结果

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14737/6248_zh-CN.png)

-   查询各城市、男性女性人群，购买的基金总额（多表Join查询）
    1.  查询SQL

        ```
        SELECT u_city, u_gender, SUM(t_confirmbalance) AS sum_balance 
        FROM tradelist_csv , userinfo  
        where u_userid = t_userid 
        GROUP BY u_city, u_gender 
        ORDER BY sum_balance DESC;
        ```

    2.  显示执行结果

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14737/6249_zh-CN.png)


## SQL查询\(异步执行\) {#section_kgq_5nt_c2b .section}

1.  异步执行查询，将查询结果以CSV格式输出到OSS。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14737/6250_zh-CN.png)

2.  点击**执行状态**，可查看该异步查询任务的执行状态。

    **说明：** 执行状态主要分为RUNNING、SUCCESS 和 FAILURE三种。点击**刷新**，当STATUS变为SUCCESS时，可以查看查询结果输出到OSS的文件路径。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14737/6251_zh-CN.png)

3.  查看导出OSS的结果文件。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14737/6252_zh-CN.png)


