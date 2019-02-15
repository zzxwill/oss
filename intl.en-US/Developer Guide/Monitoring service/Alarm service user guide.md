# Alarm service user guide {#concept_opj_kzj_5db .concept}

To help familiarize yourself with the basic concepts and configurations of alarm contacts and alarm contact groups, we recommend that the following documents are read before this user guide:


Additionally, OSS alarm rules are developed in accordance with OSS metric items. This means they are categorized by dimensions similar to those of OSS metric items. Two alarm dimensions are available: user-level and bucket-level.

## Alarm rule page {#section_nf3_4zj_5db .section}

The alarm rule page is where you can view, modify, activate, deactivate, and delete alarm rules related to OSS monitoring alarms. You can also view historical alarms of the different alarm rules. An example screenshot is as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15502083591216_en-US.png)

-   Click **Modify** next to the expected alarm rule to modify it.
-   Click **Delete** next to the expected alarm rule to delete it.  You can also select multiple alarm rules and then click **Delete** at the bottom of the table to delete alarm rules in batches.
-   If an alarm rule is in the Enable status, click **Suspend** next to the expected alarm rule to deactivate it. Once the alarm rule is suspended, you no longer receive alarm information for this rule.  You can also select multiple alarm rules and then click **Forbidden** at the bottom of the table to deactivate alarm rules in batches.
-   If an alarm rule is in the Forbidden status, click **Enable** next to the expected alarm rule to activate it. The rule is then be resumed to detect exceptions and send alarm information.  You can also select multiple alarm rules and then click **Enable** at the bottom of the table to activate alarm rules in batches.
-   Click **Alarm history** next to the expected alarm rule to view information on past alarms corresponding to this rule. 

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15502083606384_en-US.png)

Alarm history concepts

-   Alarm history refers to past status changes of a selected alarm rule. Operations such as switching from normal status to alarm status, or switching from alarm status to normal status, are considered status changes.  Additionally, a status change called channel silence is also available.
-   **Channel silence** occurs when a triggered alarm has remained active for 24 hours and has not returned to a normal status.  In this case, no new alarm notifications are sent for 24 hours.
-   Historical alarm information is retained for one month, and can be queried at a maximum of three days' data at one time within this time period.  Alarm information older than one month is automatically deleted, and cannot be queried.

To view details about an alarm, such as the alarm contact list and contact details, click **View** next to the expected alarm. An example screenshot displaying specific details is as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15502083606385_en-US.jpg)

Search for alarm rules

Based on the control information at the bottom of the alarm rule page, you can quickly find alarm rules you have searched for:

-   Alarm dimension drop-down box: All and Bucket Level.  If you select All, all user-level and bucket-level alarm rules are displayed.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15502083606386_en-US.png)

-   Bucket drop-down box: If you select Bucket Level in the alarm dimension drop-down box, this box lists the buckets of the current user.  Select a bucket to display all the alarm rules for this bucket:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15502083606387_en-US.png)

-   Monitored items drop-down box lists all OSS metric items, including user-level and bucket level metric items. If you select Monitored items, user-level and bucket-level alarm rules for all monitored items are displayed.
-   Alarm status drop-down box lists alarm status, including OK and Alarm .
-   Enable state drop-down box lists the enable status, including Enabled and Forbidden .

-   View alarm rules

    Click the **Alarm rules** tab to display all alarm rules. You can also select Bucket Level in the drop-down box and then select the name of the expected bucket to see alarm rules for that bucket. You can then filter returned information using selections in the drop-down box such as Metric Item, Alarm status and Activation status.

-   View alarm rules for a specific bucket

    If you want to view the alarm rules of a specific bucket, select Bucket Level in the alarm dimension drop-down box and then select the name of the target bucket in the bucket drop-down box. Select **Alarm Rules** for the target bucket in the **Bucket List** to go to the alarm tab. This tab displays all the alarm rules for this bucket. With the Metric item, Alarm status and Activation status drop-down boxes, you can better filter the alarm rules that match certain conditions in the current dimension.

-   View alarm rules related to a specific metric item

    Select a specific metric item in the metric item drop-down box to display all the alarm rules for this metric item.

-   View alarm rules in a certain alarm status

    Choose an alarm status in the alarm status drop-down box, such as Alarm, to display all the alarm rules currently in this status.

-   View alarm rules in a certain activation status

    Choose an activation status in the activation status drop-down box, such as Deactivated, to display all the alarm rules currently in this status.


Add alarm rules

After specifying a bucket in the Bucket List Tab, click **Set Alarm Rule** to set an alarm rule. Alternatively, click the alarm icon ![](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/image/media/alert_chart.jpg) in a metric chart in the User Overview tab or the Monitoring View tab of a specific bucket to open the **Batch Set Alarm Rules** page to set multiple alarm rules. 

1.  Set parameters for **Alarm rules** as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15502083601217_en-US.jpg)

    -   Alarm dimension specifies the monitoring dimension of the alarm rule to set. If the dimension is set to bucket-level, the expected bucket with which to set the alarm rule for must be specified.
    -   Monitored items specifies all the metric items for the selected alarm dimension. You can use the quick search box to easily find metric items:
    -   Statistics interval specifies the length of the interval between statistical measurements. The default setting is 5 minutes.
    -   Last times specifies the number of statistical cycles for which an alarm which is triggered when the value of the metric item continuously exceeds the threshold value in several consecutive statistical cycles.
    -   Statistics method: specifies the statistical indicator calculated for this metric item. For the OSS monitoring service, the statistical method is set as Monitoring Value.
    -   Click **+ Add alarm rules** to set additional metric item alarm rules. 
    -   Click **Delete** next to the expected alarm rule to delete it. 
2.  Click **Next**, the page to **Set the alarm types** is then displayed. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15502083601219_en-US.jpg)

3.  Click **OK**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4393/15502083601220_en-US.jpg)


-   Add alarm rules in the Bucket list

    Under the Bucket list tab, you can add identical alarm rules for multiple buckets at the same time. Select the expected buckets for which to configure alarm rules and click **Set Custom monitor alarm rules** to go to the alarm rule settings page previously described in Add alarm rules. 

    **Note:** During batch setting, the alarm dimension is bucket-level and the metric item must be a bucket-level metric item. 

-   Add alarm rules in a metric chart

    In the User overview or Monitoring chart tab, for the expected bucket, click ![](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/image/media/alert_chart.jpg) in the upper-right corner of a metric chart to set alarm rules for the metric item associated with this metric chart. 

    **Note:** If you click the alarm icon in a metric chart, the alarm dimension displayed on the alarms rule page is pre-determined and you can only set alarm rules for the metric item corresponding to the metric chart. 


## Considerations {#section_jt1_x2k_5db .section}

Currently, alarm rules can be created without requiring prior association to a bucket. If you delete a bucket, any associated alarm rules are not deleted. Before deleting a bucket, we recommend that you delete any corresponding alarm rules first.

