# EMR+OSS: Separated storage and computing for offline computing {#concept_x3n_yrm_vdb .concept}

## Background {#section_xkj_1sm_vdb .section}

In traditional Hadoop usage, storage and computing are inseparable. Therefore, as your business grows, the cluster size often cannot meet your needs for business expansion. For example, if your data scale exceeds the cluster’s storage capacity, the new requirements arising from the data production cycle of your business may outpace computing capabilities. In this case, you must be ready at all times to deal with the challenges of insufficient cluster storage space or computing capabilities.

If you choose to deploy computing and storage in a hybrid manner, storage scaling can often lead to excess computing capabilities. This is a waste of resources. Likewise, an increase in computing capabilities causes a waste of storage resources.

Separating computing and storage for offline computing makes it easier to cope with insufficient computing or storage resources. In this solution, you can store all your data in OSS and then analyze it using stateless E-MapReduce. Therefore, E-MapReduce is only responsible for computation, and storage resource are not tied to computing resources in your business. This approach provides the highest flexibility.

## Architecture {#section_tzg_bsm_vdb .section}

The architecture for offline computing with separated storage and computing is simple, as shown in the following figure. OSS acts as the default storage unit, and Hadoop or Spark acts as a computing engine that directly analyzes data stored in OSS.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4434/2247_en-US.png)

## Benefits {#section_igt_fsm_vdb .section}

|Factor|Integrated computing and storage|Separated computing and storage|
|:-----|:-------------------------------|:------------------------------|
|Flexibility|Not flexible|After computing and storage are separated, cluster rules are simple and flexible. You hardly need to estimate your future business scale, besides using the resources as you need.|
|Cost|High|Ultra cloud disks are used in self-built ECS systems. After separating storage and computing, if the cluster configuration is one master node with an  8-core 32 GB CPU, six slave nodes with 8-core 32 GB CPUs, and 10 TB of data, the cost is roughly halved.|
|Performance|Relatively high|At most, performance drops by 10%.|

## Test case {#section_sdx_lsm_vdb .section}

-   Test conditions

    For the detailed test code, see [GitHub](https://github.com/fengshenwu/spark-terasort/tree/master/src/main/scala/com/github/ehiggs/spark/terasort).

    Cluster scale: 1 master node with a 4-core  16 GB CPU, 8 slave nodes with 4-core 16 GB CPUs, each slave node has four 250 GB  ultra cloud disks.

    The Spark test script is as follows.

    ```
    /opt/apps/spark-1.6.1-bin-hadoop2.7/bin/spark-submit --master yarn --deploy-mode cluster --executor-memory 3G --num-executors 30 --conf spark.default.parallelism=800 --class com.github.ehiggs.spark.terasort.TeraSort spark-terasort-1.0-jar-with-dependencies.jar /data/teragen_100g /data/terasort_out_100g
    ```

-   Test results
    -   Performance

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4434/2248_en-US.png)

    -   Cost

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4434/2250_en-US.png)

    -   Time

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4434/2251_en-US.png)

-   Result analysis

    From the performance chart, we can compare the respective advantages of the EMR + OSS and self-built Hadoop with ECS systems:

    -   The overall load is lower

    -   Memory utilization is basically the same

    -   CPU usage is lower, in which case, the usage level for iowait and sys is much lower. Because the datanode and disk operations of the self-built ECS system occupy resources, this adds to the CPU overhead.

    -   In terms of network usage, because sortbenchmark performs two data read operations \(the first for sampling and the second for actually reading the data\), network usage starts out high, and then in the shuffle+ results output stage, drops to about half of the self-built Hadoop with ECS system. Therefore, from the network perspective, the overall usage is basically flat.

    In short, with EMR + OSS, the cost is halved, but the drop in performance is negligible. Moreover, an increase in the concurrency of the EMR + OSS solution means better time advantage in comparison with the self-built Hadoop with ECS system.


## Unsuitable scenarios {#section_vp5_ftm_vdb .section}

We recommend that you do not use EMR + OSS in the following scenarios:

-   Scenarios with a large number of small files

    In this case, merge files smaller than 10 MB. The EMR + OSS solution provides the best performance when data volumes exceed 128 MB.

-   Scenarios with frequent OSS metadata operations


