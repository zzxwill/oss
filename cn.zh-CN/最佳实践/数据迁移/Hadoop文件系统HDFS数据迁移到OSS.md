# Hadoop文件系统HDFS数据迁移到OSS {#concept_l42_mnw_5db .concept}

本文介绍如何快速地将 Hadoop 文件系统（HDFS）上的数据迁移到 OSS。

## 背景 {#section_e1j_xnw_5db .section}

当前业界有很多公司是以Hadoop技术构建数据中心，而越来越多的公司和企业希望将业务顺畅地迁移到云上。

在阿里云上使用最广泛的存储服务是对象存储OSS。OSS的数据迁移工具ossimport可以将您本地或第三方云存储服务上的文件同步到OSS上，但这个工具无法读取Hadoop文件系统的数据，从而发挥Hadoop分布式的特点。并且，该工具只支持本地文件，需要将HDFS上的文件先下载到本地，再通过工具上传，整个过程耗时又耗力。

阿里云E-MapReduce团队开发的Hadoop数据迁移工具**emr-tools**，能让您从Hadoop集群直接迁移数据到OSS上。

## 前提条件 {#section_mds_ynw_5db .section}

确保当前机器可以正常访问您的Hadoop集群，即能够用Hadoop命令访问HDFS。

```
hadoop fs -ls /
```

## 实施步骤 {#section_ghz_j4w_5db .section}

1.  下载[emr-tools](https://yq.aliyun.com/attachment/download/?spm=5176.100239.blogcont78093.18.BfNz7d&id=1956)。

    **说明：** emr-tools兼容Hadoop 2.4.x、2.5.x、2.6.x、2.7.x版本。

2.  解压缩工具到本地目录。

    ```
    tar jxf emr-tools.tar.bz2
    ```

3.  复制HDFS数据到OSS上。

    ```
    cd emr-tools
    ./hdfs2oss4emr.sh /path/on/hdfs oss://accessKeyId:accessKeySecret@bucket-name.oss-cn-hangzhou.aliyuncs.com/path/on/oss
    ```

    参数说明如下。

    |参数|说明|
    |:-|:-|
    |accessKeyId|访问OSS API的密钥。获取方式请参见[如何获取如何获取AccessKeyId和AccessKeySecret](https://www.alibabacloud.com/help/doc-detail/48699.htm)。

|
    |accessKeySecret|
    |bucket-name.oss-cn-hangzhou.aliyuncs.com|OSS的访问域名，包括bucket名称和endpoint地址。|

    系统将启动一个Hadoop MapReduce任务（DistCp）。

4.  运行完毕之后会显示本次数据迁移的信息。信息内容类似如下所示。

    ```
    17/05/04 22:35:08 INFO mapreduce.Job: Job job_1493800598643_0009 completed successfully
    17/05/04 22:35:08 INFO mapreduce.Job: Counters: 38
     File System Counters
             FILE: Number of bytes read=0
             FILE: Number of bytes written=859530
             FILE: Number of read operations=0
             FILE: Number of large read operations=0
             FILE: Number of write operations=0
             HDFS: Number of bytes read=263114
             HDFS: Number of bytes written=0
             HDFS: Number of read operations=70
             HDFS: Number of large read operations=0
             HDFS: Number of write operations=14
             OSS: Number of bytes read=0
             OSS: Number of bytes written=258660
             OSS: Number of read operations=0
             OSS: Number of large read operations=0
             OSS: Number of write operations=0
     Job Counters
             Launched map tasks=7
             Other local map tasks=7
             Total time spent by all maps in occupied slots (ms)=60020
             Total time spent by all reduces in occupied slots (ms)=0
             Total time spent by all map tasks (ms)=30010
             Total vcore-milliseconds taken by all map tasks=30010
             Total megabyte-milliseconds taken by all map tasks=45015000
     Map-Reduce Framework
             Map input records=10
             Map output records=0
             Input split bytes=952
             Spilled Records=0
             Failed Shuffles=0
             Merged Map outputs=0
             GC time elapsed (ms)=542
             CPU time spent (ms)=14290
             Physical memory (bytes) snapshot=1562365952
             Virtual memory (bytes) snapshot=17317421056
             Total committed heap usage (bytes)=1167589376
     File Input Format Counters
             Bytes Read=3502
     File Output Format Counters
             Bytes Written=0
     org.apache.hadoop.tools.mapred.CopyMapper$Counter
             BYTESCOPIED=258660
             BYTESEXPECTED=258660
             COPY=10
    copy from /path/on/hdfs to oss://accessKeyId:accessKeySecret@bucket-name.oss-cn-hangzhou.aliyuncs.com/path/on/oss does succeed !!!
    ```

5.  您可以用osscmd等工具查看OSS上数据情况。

    ```
    osscmd ls oss://bucket-name/path/on/oss
    ```


## OSS数据迁移到Hadoop {#section_kzs_sqw_5db .section}

如果您已经在阿里云上搭建了Hadoop集群，可以使用如下命令把数据从OSS上迁移到新的Hadoop集群。

```
./hdfs2oss4emr.sh oss://accessKeyId:accessKeySecret@bucket-name.oss-cn-hangzhou.aliyuncs.com/path/on/oss /path/on/new-hdfs
```

## 更多使用场景 {#section_swh_wqw_5db .section}

除了线下的集群，在ECS上搭建的Hadoop集群也可以使用emr-tools，将自建集群迅速地迁移到[E-MapReduce](https://www.aliyun.com/product/emapreduce?)服务上。

如果您的集群已经在ECS上，但是在经典网络中，无法和VPC中的服务做很好的互操作，所以想把集群迁移到VPC中。可以按照如下步骤迁移：

1.  使用emr-tools迁移数据到OSS上。
2.  在VPC环境中新建一个集群（自建或使用E-MapReduce服务）。
3.  将数据从OSS上迁移到新的HDFS集群中。

If you use E-MapReduce, on the Hadoop cluster, you can directly access OSS using [Spark](https://www.alibabacloud.com/help/doc-detail/28118.htm?spm=a2c63.p38356.a3.7.41d2b5c82kHPxv), [MapReduce](https://www.alibabacloud.com/help/doc-detail/28128.htm) and [Hive](https://www.alibabacloud.com/help/doc-detail/28129.htm). This not only avoids one data copy operation \(from OSS to HDFS\), but also greatly reduces storage costs. For more information about cost reduction, see [EMR+OSS: Separated storage and computing](intl.zh-CN/最佳实践/数据处理与分析/EMR+OSS：离线计算的存储与计算分离.md#).

