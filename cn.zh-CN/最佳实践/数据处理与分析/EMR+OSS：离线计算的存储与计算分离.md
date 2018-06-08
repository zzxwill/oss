# EMR+OSS：离线计算的存储与计算分离 {#concept_x3n_yrm_vdb .concept}

## 背景 {#section_xkj_1sm_vdb .section}

在传统Hadoop的使用中，存储与计算密不可分，而随着业务的发展，集群的规模常常不能满足业务的需求。例如，数据规模超过了集群存储能力，业务上对数据产出的周期提出新的要求导致计算能力跟不上。这就要求我们能随时应对集群存储空间不足或者计算能力不足的挑战。

如果将计算和存储混合部署，常常会因为为了扩存储而带来额外的计算扩容，这其实就是一种浪费；同理，只为了提升计算能力，也会带来一段时期的存储浪费。

将离线计算的计算和存储分离，可以更好地应对单方面的不足。把数据全部放在OSS中，再通过无状态的E-MapReduce分析。E-MapReduce只需进行纯粹的计算，不存在存储跟计算搭配来适应业务了，这样最为灵活。

## 架构 {#section_tzg_bsm_vdb .section}

离线计算的存储和计算分离架构简单，如下图所示。OSS作为默认的存储，Hadoop/Spark作为计算引擎直接分析OSS存储的数据。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4434/2247_zh-CN.png)

## 优势 {#section_igt_fsm_vdb .section}

|因素|计算和存储不分离|计算和存储分离|
|:-|:-------|:------|
|灵活性|不灵活|计算与存储分离后，集群规划简单灵活，基本不需要估算未来业务的规模，做到按需使用。|
|成本|高|在ECS自建的磁盘选择高效云盘，以1 master 8 cpu32g/6 slave 8 cpu32g/10T数据量为例进行估算，存储与计算分离后，成本下降一倍。|
|性能|较高|至多下降10%。|

## 案例测试 {#section_sdx_lsm_vdb .section}

-   测试条件

    详细的测试代码请参见[GitHub](https://github.com/fengshenwu/spark-terasort/tree/master/src/main/scala/com/github/ehiggs/spark/terasort)。

    集群规模：1 master 4cpu 16g、8 Slave 4cpu 16g、每个slave节点250G\*4 高效云盘

    Spark测试脚本如下所示。

    ```
    /opt/apps/spark-1.6.1-bin-hadoop2.7/bin/spark-submit  --master yarn --deploy-mode cluster --executor-memory 3G --num-executors 30    --conf spark.default.parallelism=800   --class  com.github.ehiggs.spark.terasort.TeraSort  spark-terasort-1.0-jar-with-dependencies.jar /data/teragen_100g /data/terasort_out_100g
    ```

-   测试结果
    -   性能

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4434/2248_zh-CN.png)

    -   成本![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4434/2250_zh-CN.png)
    -   时间

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4434/2251_zh-CN.png)

-   结果分析

    从性能图上看，EMR+OSS相较于ECS自建Hadoop，有如下优势：

    -   整体的负载更低。

    -   内存利用率基本一样。

    -   CPU使用低，其中iowait与sys低很多。因为ECS自建有datanode及磁盘操作，需要占一些资源，增加CPU的开销。

    -   从网络看，因为sortbenchmark有两次读取数据，第一次是采样，第二次是真正的读取数据，开始网络比较高，随后shuffle+输出结果阶段，网络比ECS自建Hadoop低一半左右。因此从网络来看，整体使用量基本持平。

    综上所述，EMR+OSS比自建ECS使用更少的资源，成本节约了一半，但是性能下降基本可以忽略不计。并且，如果提高EMR+OSS的并发度，则时间上有可能比ECS自建Hadoop集群更有优势。


## 不适用的场景 {#section_vp5_ftm_vdb .section}

以下场景不建议使用EMR+OSS：

-   小文件过多的场景。

    如果文件小于10M时，请合并小文件。当数据量在128M以上时，使用EMR+OSS的性能最佳。

-   频繁操作OSS元数据的场景。


