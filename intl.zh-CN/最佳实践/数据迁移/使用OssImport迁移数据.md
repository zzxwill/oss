# 使用OssImport迁移数据 {#concept_awy_rdg_vdb .concept}

本文介绍如何使用OssImport将数据从第三方存储（或OSS）迁移到OSS。

## 环境配置 {#section_mj4_4s4_fgb .section}

OssImport有单机模式和分布式模式两种部署方式：

-   对于小于 30TB 的小规模数据迁移，单机模式即可完成。
-   对于大规模的数据迁移，请使用分布式模式。

OssImport部署方式详细介绍请参考[说明及配置](../../../../../intl.zh-CN/常用工具/数据迁移工具ossimport/说明及配置.md#)。

假设您需要将迁移源腾讯云COS华南1（深圳）区域的500TB数据，于一周内迁移至OSS华东1（杭州）区域，您需要进行OssImport分布式环境配置：

-   开通OSS
    1.  使用您的账号创建华东1（杭州）区域的OSS Bucket。
    2.  在RAM控制台创建子帐号，并授权该子账号访问OSS的权限，并保存AccessKeyID和AccessKeySecret。
-   购买ECS

    购买OSS同区域华东1（杭州）的ECS，一般普通的2核4G机型即可，如果迁移后ECS需释放，建议按需购买ECS。

    **说明：** 若分布式部署所需的计算机数量较少时，您可以直接在本地部署；若所需计算机数量较多时，建议在ECS实例上部署。

    ECS所需数量的计算公式为：X/Y/\(Z/100\)台。其中X为需要迁移的数据量、Y为要求迁移完成的时间（天）、Z为单台ECS迁移速度Z Mbps（每天迁移约Z/100 TB数据）。假设单台ECS迁移速度达到200Mbps\(即每天约迁移2TB数据\)，则上述示例中需购买ECS 36台（即500/7/2）。

-   配置OssImport

    结合本示例中的大规模迁移需求，您需要在ECS上搭建OssImport分布式模式。有关分布式部署的配置定义信息，如`conf/job.cfg`、`conf/sys.properties`、并发控制等配置，请参考[说明及配置](../../../../../intl.zh-CN/常用工具/数据迁移工具ossimport/说明及配置.md#)。有关分布式部署的相关操作，如OssImport下载、配置过程的常见错误及排除，请参考[分布式部署](../../../../../intl.zh-CN/常用工具/数据迁移工具ossimport/分布式部署.md#)。


## 迁移步骤 {#section_bdl_ybq_fgb .section}

使用分布式模式将第三方存储迁移至OSS的过程如下：

**说明：** 在ECS上搭建OssImport分布式环境后，OssImport从腾讯云COS华南1（深圳）区域下载数据到ECS华东1（杭州），建议使用外网。使用OssImport从ECS华东1（杭州）将数据上传到OSS华东1（杭州），建议使用内网。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4431/15537559941976_zh-CN.png)

1.  全量迁移第三方存储T1前的历史数据，详细步骤请参考分布式部署的[运行](../../../../../intl.zh-CN/常用工具/数据迁移工具ossimport/分布式部署.md#section_m1y_1jh_wdb)。

    **说明：** T1为Unix时间戳，即自1970年1月1日UTC零点以来的秒数，通过命令date +%s获取。

2.  在OSS控制台打开[OSS镜像回源](../../../../../intl.zh-CN/控制台用户指南/管理存储空间/设置回源规则.md#)，回源地址设置为迁移源（第三方存储）。
3.  将业务系统读写切换至OSS，此时业务系统记录的时间为T2。

    T1前的数据从OSS读取，T1后的数据则通过OSS镜像回源从第三方存储读取，新数据完全写入OSS。

    T2后不再有新数据写入迁移源。

4.  修改配置文件`job.cfg`的配置项`importSince=T1`，重新发起迁移任务，进行T1~T2的增量数据迁移。

    **说明：** 

    -   步骤4完成后，您业务系统的所有的读写都在OSS上。第三方存储只是一份历史数据，您可以根据需要决定保留或删除。
    -   OssImport只负责数据的迁移和校验，不会删除任何数据。

## 费用说明 {#section_myw_td3_dhb .section}

迁移过程涉及到的成本包含：源和目的存储空间访问费用、源存储空间的流出流量费用、ECS实例费用、数据存储费用、时间成本。如果数据超过TB级别，存储成本和迁移时间成正比，且相对流量、存储费用，ECS费用较小，增加ECS数量，会减少迁移时间。

## 参考文档 {#section_fvm_cbv_fgb .section}

有关OssImport的相关说明，请参考以下文档：

[分布式部署](../../../../../intl.zh-CN/常用工具/数据迁移工具ossimport/分布式部署.md#)

[说明及配置](../../../../../intl.zh-CN/常用工具/数据迁移工具ossimport/说明及配置.md#)

[数据迁移](../../../../../intl.zh-CN/常用工具/数据迁移工具ossimport/数据迁移.md#)

[常见问题](../../../../../intl.zh-CN/常用工具/数据迁移工具ossimport/常见问题.md#)

