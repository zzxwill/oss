# OSS资源的监控与报警 {#concept_uyy_vym_vdb .concept}

云监控服务能够监控OSS服务资源。借助云监控服务，您可以全面了解您在阿里云上的资源使用情况、性能和运行状况。借助报警服务，您可以及时做出反应，保证应用程序顺畅运行。本章介绍如何监控OSS资源、设置报警规则以及自定义监控大盘。

## 前提条件 {#section_myf_1zm_vdb .section}

-   已开通[OSS服务](https://www.alibabacloud.com/product/oss)。
-   已开通[云监控服务](https://www.alibabacloud.com/product/cloud-monitor)。

## 监控OSS资源 {#section_iy2_jzm_vdb .section}

1.  登录[云监控控制台](https://cloudmonitor.console.aliyun.com/#/home/ecs)。
2.  在左侧导航栏选择 **云服务监控** \> **对象存储OSS**，进入OSS监控页面，如下图所示。

    在OSS监控页面，您可以获取各类监控数据。

    **说明：** 用户层级指用户级别的数据，即该用户下所有bucket的数据。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4439/2299_zh-CN.png)


## 设置报警规则 {#section_mrp_11n_vdb .section}

1.  在OSS监控页面的报警规则页签下，单击****创建报警规则****。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4439/2300_zh-CN.png)

2.  填写配置项。

    配置项说明参见[管理报警规则](https://www.alibabacloud.com/help/doc-detail/28610.htm)。

3.  完成配置后，即生成一条报警规则，您可以使用测试数据来检测该条规则是否生效，确认能否顺利接收报警信息（邮件、短信、旺旺、钉钉等）。

## 自定义监控大盘 {#section_pzd_l1n_vdb .section}

您可以参考如下步骤，在云监控控制台上自定义配置OSS资源监控图。

1.  登录[云监控控制台](https://cloudmonitor.console.aliyun.com/#/home/ecs)。
2.  在左侧导航栏单击**Dashboard**。
3.  单击**创建监控大盘**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4439/2302_zh-CN.png)

4.  输入大盘名称后，单击**添加图表**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4439/2303_zh-CN.png)

5.  根据需求完成配置，并单击**发布**。

    配置项说明参见[监控指标参考手册](../intl.zh-CN/开发指南/监控服务/监控指标参考手册.md#)。


