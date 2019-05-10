# Alert service {#concept_opj_kzj_5db .concept}

This topic describes alert rules for monitoring OSS in the CloudMonitor console and how to set such alert rules.

Before getting started with OSS alert rules, you can read the following topics to get familiar with the basic concepts of the alert service and the configurations of alert contacts and alert contact groups.

-   [Alert service overview](../../../../reseller.en-US/User Guide/Alarm service/Alarm service overview.md#)
-   [Manage alert contacts and alert contact groups](../../../../reseller.en-US/User Guide/Alarm service/Alarm contacts/Manage alarm contacts and alarm contact groups.md#)

OSS alert rules are developed based on OSS metrics. Therefore, OSS alert rules are categorized by dimensions similar to those of OSS metrics. Two alert dimensions are available: user-level and bucket-level.

## Alarm Rules tab {#section_nf3_4zj_5db .section}

The CloudMonitor console provides an [Alarm Rules](https://cloudmonitor.console.aliyun.com/#/cloud/alarmrules/oss//-----all-----/) tab for OSS monitoring and alerting. On this tab, you can view, modify, enable, disable, and delete alert rules. You can also view the historical alert information of a specific alert rule.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15574545321216_en-US.png)

-   Click **View** in the Actions column corresponding to an alert rule to view its content.
-   Click **Modify** in the Actions column corresponding to an alert rule to modify it.
-   Click **Delete** in the Actions column corresponding to an alert rule to delete it. You can also select multiple alert rules and click **Delete** below the alert rule list to delete multiple alert rules at a time.
-   If an alert rule is in the Enabled state, click **Disable** in the Actions column corresponding to the alert rule to disable it. After the alert rule is disabled, you no longer receive alert information for this rule. You can also select multiple alert rules and click **Disable** below the alert rule list to disable multiple alert rules at a time.
-   If an alert rule is in the Disabled state, click **Enable** in the Actions column corresponding to the alert rule to enable it. The alert rule takes effect again to detect exceptions and send alert information. You can also select multiple alert rules and click **Enable** below the alert rule list to enable multiple alert rules at a time.
-   Click **Alarm Logs** in the Actions column corresponding to an alert rule. You can view the information about historical alerts corresponding to this rule.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15574545326384_en-US.png)


Relevant concepts:

-   Historical alert information records the past status changes of a selected alert rule, such as a status change from OK to Alarm, a status change from Alarm to OK, and a special status change of Muted.
-   When an alert rule is in the **Muted** state, the alert triggered by this alert rule remains active within a specified period and is not cleared. During the muted period, the system does not send alert information to notification contacts until the muted period expires.
-   Historical alert information is kept for one month. Alerts generated earlier than one month are automatically deleted. You can query data of three days at most, and cannot query data generated 31 days ago.

You can click **View** in the Notification Contact column corresponding to an alert rule to view the members of notification contacts \(in alert contact groups\) and the methods that they use to receive alert information \(such as SMS message, email, or TradeManager\), as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15574545326385_en-US.jpg)

## View alert rules {#section_h25_xrt_q2b .section}

-   Locate alert rules

    You can use the GUI elements on the Alarm Rules tab to locate the alert rules that you search for.

    -   Alert dimension drop-down list: You can select All or BucketLevel. If you select All, all user-level and bucket-level alert rules appear in the alert rule list.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15574545326386_en-US.png)

    -   Bucket drop-down list: If you select BucketLevel from the alert dimension drop-down list, the bucket drop-down list displays all buckets of the current Alibaba Cloud account. You can select a bucket to view all alert rules for this bucket.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15574545326387_en-US.png)

    -   Metrics drop-down list: It displays all OSS metrics, including user-level and bucket-level metrics. If you select All, user-level or bucket-level alert rules for all metrics appear in the alert rule list.
    -   Status drop-down list: You can select a status to view all alert rules in this state, such as OK, Alarm, Insufficient Data, Enable, or Disable. If you select All, alert rules in all statuses appear in the alert rule list.
    -   \[DO NOT TRANSLATE\] \[DO NOT TRANSLATE\]
-   View all alert rules

    If you click the **Alarm Rules** tab, all alert rules automatically appear in the alert rule list. You can also select All from the alert dimension drop-down list to view all alert rules. Then, you can use the Metrics and Status drop-down lists to set filtering conditions and accurately locate alert rules in this alert dimension.

-   View alert rules for a specific bucket

    To view the alert rules for a specific bucket, you need to select BucketLevel from the alert dimension drop-down list and select the name of the destination bucket from the bucket drop-down list. You can also click the **Bucket List** tab. On the tab that appears, you can click **Alarm Rules** in the Actions column corresponding to the relevant bucket to go to the Alarm Rules tab, where you can view all alert rules for this bucket. Then, you can use the Metrics and Status drop-down lists to set filtering conditions and accurately locate alert rules in this alert dimension.

-   View alert rules related to a specific metric

    You can select a metric from the Metrics drop-down list to view all alert rules for this metric.

-   View alert rules in a certain alert state

    You can select an alert status from the Status drop-down list, such as Alarm, to view all alert rules that are in this status.

-   \[DO NOT TRANSLATE\]

    \[DO NOT TRANSLATE\]


## Add an alert rule {#section_os1_yrt_q2b .section}

1.  Use any of the following methods to go to the Create Alarm Rule page:
    -   On the [Users](https://cloudmonitor.console.aliyun.com/#/cloud/overview/oss/) tab, click **Monitoring Service Overview** and click ![](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/image/media/alert_chart.jpg) in any chart.
    -   On the [Bucket List](https://cloudmonitor.console.aliyun.com/#/cloud/buckets/oss/) tab, click the relevant bucket name to go to the bucket details page. Click **Create Alarm Rule**.
    -   On the [Bucket List](https://cloudmonitor.console.aliyun.com/#/cloud/buckets/oss/) tab, click the relevant bucket name. On the page that appears, click **Monitoring Service Overview** and click ![](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/image/media/alert_chart.jpg) in any chart.

        ![](images/38977_en-US.png)

2.  Set the alert rule as needed.
    -   **Related Resource**
        -   Products: Select Object Storage Service.
        -   Resource Range: Select All Resources or bucketDimensions as needed.
        -   Bucket \(if you set Resource Range to bucketDimensions\): Select one or more buckets as needed.
    -   **Set Alarm Rules**
        -   Alarm Rule: Enter the alert rule name.
        -   Rule Describe: Select the content, time, and threshold of the metric as needed.
        -   **+Add Alarm Rule**: Click it to add more alert rules.
        -   Mute for: Specify the interval for sending an alert notification if the exception persists after the alert is triggered.
        -   Triggered when threshold is exceeded for: Specify the times that the rule is matched consecutively to send alert notifications. For example, if you select Internet Outbound Traffic, 1mins, Value, \>, and 100 for Rule Describe and set Triggered when threshold is exceeded for to 3, an alert is triggered only when the Internet outbound traffic exceeds 100 MB three consecutive times within 1 minute.
        -   Effective Period: Select the time the alert rule takes effect.
    -   **Notification Method**
        -   **Notification Contact**: If you have set an alert contact group by following the procedure in [Manage alert contacts and alert contact groups](../../../../reseller.en-US/User Guide/Alarm service/Alarm contacts/Manage alarm contacts and alarm contact groups.md#), select the group. If you have not set any alert contact groups, click **Quickly create a contact group** to create a group by following the instructions.
        -   Notification Methods: Select notification methods for the alert rule.****[https://common-buy.aliyun.com/?commodityCode=cms\_call\_num\#/buy](https://common-buy.aliyun.com/?commodityCode=cms_call_num#/buy)
        -   Email Subject: Enter the subject of the notification email.
        -   Email Remark: optional. Enter the remarks of the email.
        -   HTTP CallBack: Enter a URL that can be accessed from the Internet. CloudMonitor sends a POST request to push the alert notification to this URL. Currently, only HTTP is supported.
3.  Click **Confirm** to complete the alert rule setting.

## Notes {#section_jt1_x2k_5db .section}

Currently, alert rules for a bucket are not associated with the existence of the bucket. If you delete a bucket, alert rules for this bucket still exist. Therefore, we recommend that you delete alert rules for a bucket before deleting this bucket.

