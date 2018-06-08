# OSS resource monitoring and alarm service {#concept_uyy_vym_vdb .concept}

The CloudMonitor service can monitor OSS resources.  You can use CloudMonitor to view resource usage, performance, and health status on Alibaba Cloud.   Using the alarm service, you can react rapidly to keep applications running smoothly. This article introduces how to monitor OSS resources, set OSS alarm rules, and create custom monitoring dashboard.

## Prerequisites {#section_myf_1zm_vdb .section}

-   Activate the [OSS service](https://www.alibabacloud.com/product/oss).
-   Activate the [CloudMonitor service](https://www.alibabacloud.com/product/cloud-monitor).

## Monitor OSS resources {#section_iy2_jzm_vdb .section}

1.  Log on to the[CloudMonitor console](https://cloudmonitor.console.aliyun.com/#/home/ecs).
2.  Select **Cloud Service Monitoring** \> **Object Storage Service**from the left-side navigation pane to enter the OSS monitoring page, as shown in the following figure.

    You can obtain monitoring data on the OSS monitoring page.

    **Note:** “by User” refers to user-level data, that is, all bucket data of this user.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4439/2299_en-US.png)


## Set alarm rules {#section_mrp_11n_vdb .section}

1.  Find the Alarm Rules tab on OSS monitoring page, and then click****Create Alarm Rule****.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4439/2300_en-US.png)

2.  Configure your alarm rules.

    For configuration details, see [Manage alarm rules](https://www.alibabacloud.com/help/doc-detail/28610.htm).

3.  The alarm rule is generated when the configuration is completed. You can use test data to check whether the rule has taken effect by verifying if the alarm information was received successfully \(over email, SMS, Trademanager, or DingTalk\).

## Custom monitoring dashboard {#section_pzd_l1n_vdb .section}

You can customize the OSS resource monitoring map on the CloudMonitor Console. The procedure is as follows.

1.  Log on to the [CloudMonitor console](https://cloudmonitor.console.aliyun.com/#/home/ecs).
2.  Click **Dashboard**from the left-side navigation pane.
3.  Click **Create Dashboard**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4439/2302_en-US.png)

4.  Enter the name of dashboard, and then click **Add View**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4439/2303_en-US.png)

5.  Configure tables as required, and then click **Save**.

    For configuration details, see [Monitoring indicators reference](../intl.en-US/Developer Guide/Monitoring Services/Monitoring indicators reference.md#).


