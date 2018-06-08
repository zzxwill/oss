# 使用OSS中的数据作为机器学习的训练样本 {#concept_kf3_dvl_vdb .concept}

## 背景 {#section_vjt_2vl_vdb .section}

-   对象存储服务OSS

    阿里云对象存储服务（Object Storage Service，以下简称 OSS），是阿里云提供的海量、安全、低成本、高可靠的云存储服务。

-   云机器学习平台PAI

    阿里云机器学习PAI（Platform of Artificial Intelligence，以下简称PAI）是一款一站式的机器学习平台，包含大量封装的算法组件和可视化工具，用户上手容易。

-   业务场景

    本文通过OSS与PAI的结合，为一家传统的文具零售店提供决策支持。本文涉及的具体业务场景（场景与数据均为虚拟）如下：

    一家传统的线下文具零售店，希望通过数据挖掘寻找强相关的文具品类，帮助合理调整文具店的货架布局。但由于收银设备陈旧，是一台使用XP系统的POS收银机，可用的销售数据仅有一份从POS收银机导出的订单记录（csv格式）。本文介绍如何将此csv文件导入OSS，并连通OSS与PAI，实现商品的关联推荐。


## 步骤 {#section_zbf_fwl_vdb .section}

1.  数据导入OSS
    1.  新建一个名为“oss-pai-sample”的Bucket。
    2.  记录其endpoint为 oss-cn-shanghai.aliyuncs.com。
    3.  选择存储类型为标准存储。

        **说明：** OSS中有三种存储类型，相关介绍请参见[存储类型介绍](../cn.zh-CN/开发指南/存储类型/存储类型介绍.md#)。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2183_zh-CN.png)

    4.  单击Bucket名称（oss-pai-sample），然后依次单击**文件管理** \> **上传文件**，将订单数据Sample\_superstore.csv上传至OSS。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2199_zh-CN.png)

    5.  上传成功，界面如下图所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2200_zh-CN.png)

2.  创建机器学习项目
    1.  在控制台页面左侧选择**机器学习**，单击右上角的**创建项目**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2202_zh-CN.png)

    2.  在显示的DataWorks新用户引导界面中，勾选region（本文中选择与OSS相同的region：华东2），并勾选计算引擎服务机器学习PAI，然后单击**下一步**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2203_zh-CN.png)

    3.  项目创建成功后，开通服务列中会显示MaxCompute和机器学习PAI两个图标，如下图所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2230_zh-CN.png)

    4.  回到机器学习页面，点击**进入机器学习**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2209_zh-CN.png)

3.  连通OSS与PAI
    1.  在机器学习界面左侧选择**组件**，并将**OSS数据同步**组件拖拽至画布。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2210_zh-CN.png)

        界面右侧会提示填入组件需要的以下信息：

        -   OSS endpoint：根据步骤一中记录的信息，endpoint 为 oss-cn-shanghai.aliyuncs.com。
        -   OSSaccessId 和 OSSaccessKey 可以在对象存储OSS的界面中获取，如下图所示：

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2211_zh-CN.png)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2212_zh-CN.png)

        -   OSSbucket 和 object 分别为 oss-pai-sample 和 Sample\_superstore.csv。
        -   OSScolumn 映射的作用是为OSS中的csv文件增加列名。例如，虚拟数据Sample\_superstore.csv共有如下6列：

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2213_zh-CN.png)

            则OSScolumn映射应该填入：0:order\_id,1:order\_date,2:customer\_id,3:item,4:sales,5:quantity

    2.  单击**运行**，成功后右键查看组件，可观察前100条数据，如下图所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2217_zh-CN.png)

        此时OSS中的csv文件已经在MaxCompute中生成一张临时表：pai\_temp\_116611\_1297076\_1

        至此，本案例最关键的步骤已经完成，OSS中的数据已经与PAI连通，可以作为机器学习的样本进行训练。


## 数据探索流程 {#section_nwr_pqm_vdb .section}

本文所用的主要算法组件为协同过滤。有关该组件的详细用法，请参见[协同过滤做商品推荐](https://help.aliyun.com/document_detail/42600.html)。

本案例中的数据探索流程如下：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2236_zh-CN.png)

本案例按8:2的比例将源数据拆分为训练集和测试集，其中一个订单中可能有多个item，故ID列选择order\_id，保证含有多个item的订单不会被拆分，如下图所示：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2241_zh-CN.png)

本案例中共有17个产品item。通过协同过滤算法组件，取相似度最高的item，结果如下表：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4436/2242_zh-CN.png)

## 结论 {#section_bpv_lrm_vdb .section}

通过机器学习，我们发现“纸张”与“订书器”二者的相似度较高，且与其它产品也有较高的相似度。

对于这家文具零售店来说，根据此数据发现可以有两种布局货架的方式：

-   纸张和订书器货架放在最中间，其它产品货架呈环形围绕二者摆放，这样无论顾客从哪个货架步入，都可以快速找到关联程度较高的纸张和订书器。

-   将纸张和订书器两个货架分别摆放在文具店的两端，顾客需要横穿整个文具店才可以购买到另外一样，中途路过其他产品的货架可以提高交叉购买率。当然，此布局方式牺牲了用户购物的便利性，实际操作中应保持慎重。


