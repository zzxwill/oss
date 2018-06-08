# 使用OssImport迁移数据 {#concept_awy_rdg_vdb .concept}

本文向您介绍如何使用OssImport将数据从第三方存储（或OSS）迁移到OSS。

## 工具选择：单机模式和分布式模式 {#section_pnx_ydg_vdb .section}

OssImport有单机模式和分布式模式两种部署方式。一般建议使用分布式模式。您参考OssImport官网指导文档，即可完成迁移过程。本文介绍您在整体迁移方案中可能会关注的内容，以及可以参考的官网文档资源。

## 迁移方案（从第三方存储迁移到OSS） {#section_dh2_12g_vdb .section}

以下步骤可以完成从其它存储到OSS的无缝切换（参考官网支持的第三方存储类型[OssImport 架构及配置](../cn.zh-CN/常用工具/ossimport/说明及配置.md#)）：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4431/1976_zh-CN.png)

具体步骤如下：

1.  全量迁移T1前的历史数据，请参考：[OssImport 架构及配置](../cn.zh-CN/常用工具/ossimport/说明及配置.md#)。
    -   记录迁移开始时间T1（注意为Unix时间戳，即自1970年1月1日UTC零点以来的秒数，通过命令`date +%s`获取）。
    -   迁移指导说明参考OssImport官网文档，请参考[迁移工具-分布式](../cn.zh-CN/常用工具/ossimport/分布式部署.md#)。
2.  打开OSS镜像回源，并将读写切换到OSS，迁移源不再新增数据。
    -   步骤1迁移完成后，在OSS控制台打开[OSS镜像回源功能](../cn.zh-CN/控制台用户指南/管理存储空间/设置回源规则.md#)，回源地址为迁移源（第三方存储）。
    -   在业务系统读写切换到OSS，假设业务系统修改好的时间为T2。
    -   此时T1前的数据从OSS读取，T1后的数据，OSS利用镜像回源从第三方服务读取，而新数据完全写入OSS。
3.  快速迁移T1~T2到数据。
    -   在步骤2完成后，第三方存储不会再新增数据，数据读写已切到OSS。
    -   修改配置文件`job.cfg`的配置项`importSince=T1`，新发起迁移job，迁移T1~T2数据。
4.  步骤3完成后，即完成迁移全过程。
    -   步骤3完成后，您业务的所有的读写都在OSS上，第三方存储只是一份历史数据，您可以根据需要决定保留或删除。
    -   OssImport负责数据的迁移和校验，不会删除任何数据。

迁移成本

迁移过程涉及到成本一般有ECS费用、流量费用、存储费用、时间成本。其中，多数情况下，比如数据超过TB级别，存储成本和迁移时间成正比，而ECS费用相对流量、存储费用较小。

环境准备

假设您有如下迁移需求：

|需求项|需求情况|
|:--|:---|
|迁移源|AWS S3东京|
|迁移目的|OSS香港|
|数据量|500TB|
|迁移时间要求|1周内完成迁移|

您需要准备的环境：

|环境项|说明|
|:--|:-|
|开通OSS|开通OSS步骤如下：1.  使用您的账号创建香港地域的OSS Bucket。
2.  在RAM控制台创建子帐号，并授权该子账号可访问OSS。保存AccessKeyID和AccessKeySecret。

|
|购买ECS|购买OSS同区域\(香港\)的ECS，一般普通的2核4G机型即可，如果迁移后ECS需释放，建议按需购买ECS。正式迁移时的ECS数量，请参考[关于迁移用的ECS数量](#ecs1)。|
|配置OssImport| **说明：** [destDomain](../cn.zh-CN/常用工具/ossimport/说明及配置.md#)参数，即OSS目的endpoint，建议设置为OSS内网的endpoint，避免从ECS数据上传到OSS时产生外网流量费用。具体的endpoint，请参考[访问域名和数据中心](../cn.zh-CN/开发指南/访问域名和数据中心.md#)。

 |
|迁移过程| 1.  在ECS上搭建OssImport分布式环境。
2.  使用OssImport从东京AWS S3下载数据到ECS（香港），建议使用外网。
3.  使用OssImport从ECS（香港）将数据上传到OSS（香港），建议使用内网。

 |

关于迁移用的ECS数量

您需根据迁移需求，计算您需要用来做迁移的ECS数量：

-   假设您需要迁移的数据量是X TB，要求迁移完成时间Y天，单台迁移速度Z Mbps（每天迁移约Z/100 TB数据）。

-   则正式迁移时，ECS大致需要X/Y/\(Z/100\)台。


假设单台ECS迁移速度达到200Mbps\(即每天约迁移2TB数据\)。则上面Case中，ECS约需36台（500/7/2）。

## OssImport迁移步骤 {#section_rcn_f3g_vdb .section}

配置参考

您可以阅读官网指导文档，了解配置定义[OssImport 架构及配置](../cn.zh-CN/常用工具/ossimport/说明及配置.md#)、操作步骤[分布式部署](../cn.zh-CN/常用工具/ossimport/分布式部署.md#)，在开始前请关注如下信息：

-   OssImport下载：在Master下载OssImport分布式版，且master、workers都使用同样的ssh账号、密码。（worker上不用单独下载OssImport，运行`deploy`命令时，OssImport会分发到worker上，请参考[分布式部署](../cn.zh-CN/常用工具/ossimport/分布式部署.md#)。）

-   Java环境：确认Master和worker都已安装。

    **说明：** 作为worker的ECS也需要安装Java。

-   设置workdir：通过`conf/sys.properties`的配置项`workingDir`指定，请参考[分布式部署](../cn.zh-CN/常用工具/ossimport/分布式部署.md#)。

    **说明：** workdir的设置请参考官网文档，不要设置成OssImport包所在的路径，同时尽量不要设置为已有内容的目录。

-   并发控制：`conf/job.cfg`的配置项`taskObjectCountLimit`，`conf/sys.properties`的配置项`workerTaskThreadNum`，请参考[OssImport 架构及配置](../cn.zh-CN/常用工具/ossimport/说明及配置.md#)。

    如果小文件比较多，单台ECS迁移速度上不去且CPU load不高，可以参考调高`workerTaskThreadNum`、调低`taskObjectCountLimit`查看效果。

-   其他操作过程遇到问题，一般都是配置问题，请参考[分布式部署](../cn.zh-CN/常用工具/ossimport/分布式部署.md#)，并可以查看master和worker上`workdir/Logs`的日志文件。

前期测试

建议您先搭建小型环境（比如2~3台ECS），迁移少量数据以验证配置正确与否。单台ECS迁移带宽能否达到预期，比如200Mbps，如您对迁移时间明确无要求，可不关注。

带宽查看：您可使用`iftop`或`Nload`（需先安装，如`yum install ***`），ECS控制台带宽统计有延时。

**说明：** 如果测试时文件数目较少，可能无法验证并发性，可以减少参数`taskObjectCountLimit`，比如减少到：文件数/workerTaskThreadNum/worker总数。

## 迁移步骤 {#section_hdz_pkg_vdb .section}

1.  迁移历史数据。
    -   清任务、清配置。
    -   操作过程请参考[分布式部署](../cn.zh-CN/常用工具/ossimport/分布式部署.md#)。
    -   开始迁移。
    -   您可在[OSS控制台](https://oss.console.aliyun.com/overview)OSS Bucket中查看实际迁移完成的数据。
2.  设置镜像回源，客户业务系统读写切到OSS。
    -   在[OSS控制台](https://oss.console.aliyun.com/)打开[OSS镜像回源](../cn.zh-CN/控制台用户指南/管理存储空间/设置回源规则.md#)，回源地址为迁移源（第三方存储）。
    -   客户业务系统读写切换到OSS，假设业务系统修改好的时间为T2，则T2后不再有新数据写到迁移源。
3.  迁移剩余的数据。

    修改`job.cfg`配置项`importSince=T1`，请参考[OssImport 架构及配置](../cn.zh-CN/常用工具/ossimport/说明及配置.md#)，迁移剩余数据（T1~T2）特殊情况下，也可以使用`job.cfg`中`importSince = 0, isSkipExistFile=true`进行再次迁移，请参考[OssImport 架构及配置](../cn.zh-CN/常用工具/ossimport/说明及配置.md#)。


关于迁移速度

-   单台迁移速度：如单台迁移速度不理想（比如小于200Mbps、且CPU load不高时），可参考官网文档和上文，优化并发控制参数，即`conf/job.cfg`的配置项`taskObjectCountLimit`，`conf/sys.properties`的配置项`workerTaskThreadNum`。

-   迁移ECS数量：参考[ECS数量](#ecs1)估算（相对于流量、存储、时间成本，ECS费用，在迁移总成本中占比较少）。加大ECS数量，会减少迁移时间。


## OssImport分布式相关官网文档 {#section_whk_ykg_vdb .section}

|序号|官网文档|
|:-|:---|
|1|[OssImport 分布式部署](../cn.zh-CN/常用工具/ossimport/分布式部署.md#)|
|2|[OssImport 架构及配置](../cn.zh-CN/常用工具/ossimport/说明及配置.md#)|
|3|[OssImport 最佳实践](../cn.zh-CN/常用工具/ossimport/数据迁移.md#)|
|4|[OssImport 常见问题](../cn.zh-CN/常用工具/ossimport/常见问题.md#)|

