# 通过HDP2.6 Hadoop读取和写入OSS数据 {#concept_xdd_lcr_wfb .concept}

HDP（Hortonworks Data Platform） 是由 Hortonworks 发行的大数据平台，包含了 Hadoop、Hive、HBase 等开源组件。HDP 最新版本3.0.1 中的 Hadoop3.1.1 版本已经支持 OSS，但是低版本的 HDP 不支持 OSS。本文以 HDP2.6.1.0 版本为例，介绍如何配置 HDP2.6 版本支持读写 OSS。

## 准备工作 {#section_nhx_mcr_wfb .section}

您需要拥有一个已搭建好的 HDP2.6.1.0 的集群。若没有已搭建好的 HDP2.6.1.0 集群，您可以通过以下方式搭建：

-   查找参考文档利用 Ambari 搭建 HDP2.6.1.0 的集群。
-   不使用 Ambari，自行搭建 HDP2.6.1.0 集群。

## 配置步骤 {#section_rcb_ydr_wfb .section}

1.  下载 [HDP2.6.1.0 版本支持 OSS](http://gosspublic.alicdn.com/hadoop-spark/hadoop-oss-hdp-2.6.1.0-129.tar.gz)的支持包。

    **说明：** 此支持包是根据[HDP2.6.1.0](https://github.com/hortonworks/hadoop-release/tree/HDP-2.6.1.0-tag)中 Hadoop 的版本，打了 Apache Hadoop 对 OSS 支持的补丁后编译得到的，其他 HDP2 的小版本对 OSS 的支持将陆续提供。

2.  将下载的支持包解压：

    ```
     [root@hdp-master ~]# tar -xvf hadoop-oss-hdp-2.6.1.0-129.tar
    hadoop-oss-hdp-2.6.1.0-129/
    hadoop-oss-hdp-2.6.1.0-129/aliyun-java-sdk-ram-3.0.0.jar
    hadoop-oss-hdp-2.6.1.0-129/aliyun-java-sdk-core-3.4.0.jar
    hadoop-oss-hdp-2.6.1.0-129/aliyun-java-sdk-ecs-4.2.0.jar
    hadoop-oss-hdp-2.6.1.0-129/aliyun-java-sdk-sts-3.0.0.jar
    hadoop-oss-hdp-2.6.1.0-129/jdom-1.1.jar
    hadoop-oss-hdp-2.6.1.0-129/aliyun-sdk-oss-3.0.0.jar
    hadoop-oss-hdp-2.6.1.0-129/hadoop-aliyun-2.7.3.2.6.1.0-129.jar
    
    
    ```

3.  将 hadoop-aliyun-2.7.3.2.6.1.0-129.jar 移至 $\{/usr/hdp/current\}/hadoop-client/ 目录内，其余的jar文件移至$\{/usr/hdp/current\}/hadoop-client/lib/ 目录内。调整后，目录结构如下：

    ```
    [root@hdp-master ~]# ls -lh /usr/hdp/current/hadoop-client/hadoop-aliyun-2.7.3.2.6.1.0-129.jar
    -rw-r--r-- 1 root root 64K Oct 28 20:56 /usr/hdp/current/hadoop-client/hadoop-aliyun-2.7.3.2.6.1.0-129.jar
    
    [root@hdp-master ~]# ls -ltrh /usr/hdp/current/hadoop-client/lib
    total 27M
    ......
    drwxr-xr-x 2 root root 4.0K Oct 28 20:10 ranger-hdfs-plugin-impl
    drwxr-xr-x 2 root root 4.0K Oct 28 20:10 ranger-yarn-plugin-impl
    drwxr-xr-x 2 root root 4.0K Oct 28 20:10 native
    -rw-r--r-- 1 root root 114K Oct 28 20:56 aliyun-java-sdk-core-3.4.0.jar
    -rw-r--r-- 1 root root 513K Oct 28 20:56 aliyun-sdk-oss-3.0.0.jar
    -rw-r--r-- 1 root root  13K Oct 28 20:56 aliyun-java-sdk-sts-3.0.0.jar
    -rw-r--r-- 1 root root 211K Oct 28 20:56 aliyun-java-sdk-ram-3.0.0.jar
    -rw-r--r-- 1 root root 770K Oct 28 20:56 aliyun-java-sdk-ecs-4.2.0.jar
    -rw-r--r-- 1 root root 150K Oct 28 20:56 jdom-1.1.jar
    ```

    **说明：** 本文中所有 $\{\} 的内容为环境变量，请根据您实际的环境修改。

4.  在所有的 HDP 节点执行以上操作。
5.  通过 Ambari 来增加配置。没有使用 Ambari 管理的集群，可以修改core-site.xml。这里以 Ambari 为例，需要增加如下配置:![](images/32796_zh-CN.jpeg)

    |配置项|值|
    |:--|:-|
    |**fs.oss.endpoint**|填写需要连接的 OSS 的 Endpoint。例如：oss-cn-zhangjiakou-internal.aliyuncs.com

|
    |**fs.oss.accessKeyId**|填写 OSS 的 AccessKeyId。|
    |**fs.oss.accessKeySecret**|填写 OSS 的 AccessKeySecret。|
    |**fs.oss.impl**|Hadoop OSS 文件系统实现类。目前固定为：org.apache.hadoop.fs.aliyun.oss.AliyunOSSFileSystem|
    |**fs.oss.buffer.dir**|填写临时文件目录。建议值：/tmp/oss

|
    |**fs.oss.connection.secure.enabled**|是否开启 HTTPS。开启 HTTPS 会影响性能。建议值：false

|
    |**fs.oss.connection.maximum**|与 OSS 的连接数建议值：2048

|

    更多参数解释可参考 [Hadoop-Aliyun module](https://github.com/apache/hadoop/blob/trunk/hadoop-tools/hadoop-aliyun/src/site/markdown/tools/hadoop-aliyun/index.md)。

6.  根据 Ambari 提示重启集群。
7.  测试读写 OSS：
    -   测试读：

        ```
        hadoop fs -ls oss://${your-bucket-name}/
        ```

    -   测试写：

        ```
        hadoop fs -mkdir oss://${your-bucket-name}/hadoop-test
        ```

        若测试可以读写 OSS，则配置成功；若无法读写 OSS，请检查配置。

8.  为了能够运行 MAPREDUCE 任务，还需更改hdfs://hdp-master:8020/hdp/apps/2.6.1.0-129/mapreduce/mapreduce.tar.gz包的内容，如果是 TEZ 类型的作业，则修改hdfs://hdp-master:8020/hdp/apps/2.6.1.0-129/tez/tez.tar.gz，其他类型的作业以此类推。将 OSS 的支持包放如上述压缩包，执行如下命令：

    ```
    [root@hdp-master ~]# sudo su hdfs
    [hdfs@hdp-master root]$ cd
    [hdfs@hdp-master ~]$ hadoop fs -copyToLocal /hdp/apps/2.6.1.0-129/mapreduce/mapreduce.tar.gz .
    [hdfs@hdp-master ~]$ hadoop fs -rm /hdp/apps/2.6.1.0-129/mapreduce/mapreduce.tar.gz
    [hdfs@hdp-master ~]$ cp mapreduce.tar.gz mapreduce.tar.gz.bak
    [hdfs@hdp-master ~]$ tar zxf mapreduce.tar.gz
    [hdfs@hdp-master ~]$ cp /usr/hdp/current/hadoop-client/hadoop-aliyun-2.7.3.2.6.1.0-129.jar hadoop/share/hadoop/tools/lib/
    [hdfs@hdp-master ~]$ cp /usr/hdp/current/hadoop-client/lib/aliyun-* hadoop/share/hadoop/tools/lib/
    [hdfs@hdp-master ~]$ cp /usr/hdp/current/hadoop-client/lib/jdom-1.1.jar hadoop/share/hadoop/tools/lib/
    [hdfs@hdp-master ~]$ tar zcf mapreduce.tar.gz hadoop
    [hdfs@hdp-master ~]$ hadoop fs -copyFromLocal mapreduce.tar.gz /hdp/apps/2.6.1.0-129/mapreduce/
    ```


## 验证配置 {#section_llq_1jr_wfb .section}

可通过测试 teragen 和 terasort，来检测配置是否生效。

-   测试 teragen：

    ```
    [hdfs@hdp-master ~]$ hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=100 10995116 oss://{bucket-name}/1G-input
    18/10/28 21:32:38 INFO client.RMProxy: Connecting to ResourceManager at cdh-master/192.168.0.161:8050
    18/10/28 21:32:38 INFO client.AHSProxy: Connecting to Application History server at cdh-master/192.168.0.161:10200
    18/10/28 21:32:38 INFO aliyun.oss: [Server]Unable to execute HTTP request: Not Found
    [ErrorCode]: NoSuchKey
    [RequestId]: 5BD5BA7641FCE369BC1D052C
    [HostId]: null
    18/10/28 21:32:38 INFO aliyun.oss: [Server]Unable to execute HTTP request: Not Found
    [ErrorCode]: NoSuchKey
    [RequestId]: 5BD5BA7641FCE369BC1D052F
    [HostId]: null
    18/10/28 21:32:39 INFO terasort.TeraSort: Generating 10995116 using 100
    18/10/28 21:32:39 INFO mapreduce.JobSubmitter: number of splits:100
    18/10/28 21:32:39 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1540728986531_0005
    18/10/28 21:32:39 INFO impl.YarnClientImpl: Submitted application application_1540728986531_0005
    18/10/28 21:32:39 INFO mapreduce.Job: The url to track the job: http://cdh-master:8088/proxy/application_1540728986531_0005/
    18/10/28 21:32:39 INFO mapreduce.Job: Running job: job_1540728986531_0005
    18/10/28 21:32:49 INFO mapreduce.Job: Job job_1540728986531_0005 running in uber mode : false
    18/10/28 21:32:49 INFO mapreduce.Job:  map 0% reduce 0%
    18/10/28 21:32:55 INFO mapreduce.Job:  map 1% reduce 0%
    18/10/28 21:32:57 INFO mapreduce.Job:  map 2% reduce 0%
    18/10/28 21:32:58 INFO mapreduce.Job:  map 4% reduce 0%
    ...
    18/10/28 21:34:40 INFO mapreduce.Job:  map 99% reduce 0%
    18/10/28 21:34:42 INFO mapreduce.Job:  map 100% reduce 0%
    18/10/28 21:35:15 INFO mapreduce.Job: Job job_1540728986531_0005 completed successfully
    18/10/28 21:35:15 INFO mapreduce.Job: Counters: 36
    ...
    ```

-   测试 terasort：

    ```
    [hdfs@hdp-master ~]$ hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=100 oss://{bucket-name}/1G-input oss://{bucket-name}/1G-output
    18/10/28 21:39:00 INFO terasort.TeraSort: starting
    ...
    18/10/28 21:39:02 INFO mapreduce.JobSubmitter: number of splits:100
    18/10/28 21:39:02 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1540728986531_0006
    18/10/28 21:39:02 INFO impl.YarnClientImpl: Submitted application application_1540728986531_0006
    18/10/28 21:39:02 INFO mapreduce.Job: The url to track the job: http://cdh-master:8088/proxy/application_1540728986531_0006/
    18/10/28 21:39:02 INFO mapreduce.Job: Running job: job_1540728986531_0006
    18/10/28 21:39:09 INFO mapreduce.Job: Job job_1540728986531_0006 running in uber mode : false
    18/10/28 21:39:09 INFO mapreduce.Job:  map 0% reduce 0%
    18/10/28 21:39:17 INFO mapreduce.Job:  map 1% reduce 0%
    18/10/28 21:39:19 INFO mapreduce.Job:  map 2% reduce 0%
    18/10/28 21:39:20 INFO mapreduce.Job:  map 3% reduce 0%
    ...
    18/10/28 21:42:50 INFO mapreduce.Job:  map 100% reduce 75%
    18/10/28 21:42:53 INFO mapreduce.Job:  map 100% reduce 80%
    18/10/28 21:42:56 INFO mapreduce.Job:  map 100% reduce 86%
    18/10/28 21:42:59 INFO mapreduce.Job:  map 100% reduce 92%
    18/10/28 21:43:02 INFO mapreduce.Job:  map 100% reduce 98%
    18/10/28 21:43:05 INFO mapreduce.Job:  map 100% reduce 100%
    ^@18/10/28 21:43:56 INFO mapreduce.Job: Job job_1540728986531_0006 completed successfully
    18/10/28 21:43:56 INFO mapreduce.Job: Counters: 54
    ...
    ```


测试成功，配置生效。

## 参考文档 {#section_r3x_mcr_wfb .section}

关于 Hadoop 更多内容，可参考：[Hadoop 支持集成 OSS](https://yq.aliyun.com/articles/292792?spm=a2c4e.11155435.0.0.7ccba82fbDwfhK)。

您也可以通过阿里云 EMR 访问 OSS。阿里云 EMR 基于开源生态，包括 Hadoop、Spark、Kafka、Flink、Storm 等组件，为您提供集群、作业、数据管理等服务的一站式企业大数据平台，并无缝支持 OSS。阿里云 EMR 与 OSS 紧密结合，针对开源生态访问 OSS，有多项技术优化，详情可参考 [EMR产品介绍](https://www.aliyun.com/product/emapreduce))。

