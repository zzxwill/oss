# Monitoring service user guide {#concept_ods_34j_5db .concept}

## OSS monitoring entry {#section_rpm_k4j_5db .section}

The OSS monitoring service is available on the Alibaba Cloud Console. You can access the OSS monitoring service in either of the following ways:

-   Log on to your Alibaba Cloud account, and then on the homepage navigate to Object Storage Service in the left-side navigation pane. Open the OSS overview page, and then click Monitor Panel on the right side, as shown in the following picture:
-   Log on to your Alibaba Cloud account, and then on the homepage navigate to CloudMonitor in the left-side navigation pane. Open the CloudMonitor overview page, and then click Cloud Service Monitoring \> OSS as shown in the following picture:

## OSS monitoring page {#section_ejs_r4j_5db .section}

The OSS monitoring page consists of the following three tabs:

-   User overview
-   Bucket list
-   Alarm rules

The OSS monitoring page does not support automatic refresh. You can click **Refresh** in the upper-right corner to display the latest data.

Click **Go to OSS Console** to log on to the OSS console.

User overview

The User overview page displays the following information: - User monitoring information - Latest month statistics - User-level metric indicators

-   User monitoring information

    This module shows the total number of your buckets and related alarm rules.

    -   -   The parameters are as follows: Click the number next to **Bucket number** to display all the buckets you have created.
-   Click the number next to Alarm rules amount, In Alarm, Forbidden amount, or Alerted to display the following information: **Alarm rules amount** refers to total number of alarm rules you have set.
-   **In Alarm** refers to alarms in alarm state.
-   **Forbidden amount** refers to alarms that are currently disabled.
-   Alerted refers to alarms recently changed to alarm state
-   Latest month statistics

    This module displays information about charged OSS resources that you have used during the period from 00:00 on the first day of the current month, to the metering acquisition cutoff time. The following indicators are displayed:

    -   Storage utilization
    -   Internet traffic
    -   Put request
    -   Get request
    The unit of each value is automatically adjusted by the order of magnitude. The exact value is displayed when you hover the cursor over the selected value.

-   User-level metric indicators

    This module displays user-level metric charts and tables and consists of Service overview and request Status Category:

    You can select a pre-determined time range, or define a time range in the custom time boxes, to display the corresponding metric chart or table.

    -   The following time range options are available: 1 hour, 6 hours, 12 hours, 1 day, and 7 days. The default option is 1 hour.
    -   The custom time boxes allow the start time and the end time to be defined at the minute-level.
    Metric charts/tables support the following display modes:

    -   Legend hiding: You can click a legend to hide the corresponding indicator curve, as shown in the following figure:
    -   Click the ![](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/image/media/zoom_chart.jpg) icon in the upper-right corner of a metric chart to zoom in on the chart. Note: Tables cannot be zoomed in.
    -   Click the ![](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/image/media/alert_chart.jpg) icon in the upper-right corner of a metric chart to configure alarm rules for the displayed metric indicators. For more information, see the[Alarm Service User Guide](intl.en-US/Developer Guide/Monitoring Services/Alarm service user guide.md#) Note: You cannot set alarm rules for tables and metering reference indicators.
    -   Place the cursor inside the curve area of a chart, and press and hold the left button on the mouse while dragging the mouse to extend the time range. Click **Reset Zoom** to restore the original time range.
-   Service overview

    The Service overview page displays the following main metric charts:

    -   User-level availability/valid request rate, which includes two metric indicators: availability and percentage of valid requests
    -   User-level requests/valid requests, which includes two metric indicators: total number of requests and number of valid requests
    -   User-level traffic, which includes eight metric indicators: Internet outbound traffic, Internet inbound traffic, intranet outbound traffic, intranet inbound traffic, CDN outbound traffic, CDN inbound traffic, outbound traffic of cross-region replication, and inbound traffic of cross-region replication
    -   User-level request state distribution, which is a table that displays the number and percentage of each type of requests within the selected time range.
    ![](images/4334_en-US.jpg)

-   Request status category

    The \*\*Request status category\*\* page displays the metric data of request state distribution through the following metric charts:

    -   User service error request count
    -   User server error rate
    -   User network error count
    -   User network error Rate
    -   Client error request, which includes four metric indicators: number of error requests indicating resource not found, number of authorization error requests, number of client-site time-out error requests, and number of other client-site error requests
    -   Client error percent, which includes four metric indicators: percentage of error requests indicating resource not found, percentage of authorization error requests, percentage of client-site time-out error requests, and percentage of other client-site error requests
    -   User success request, which includes two metric indicators: number of successful requests and number of redirect requests
    -   User request rate, which includes two metric indicators: percentage of successful requests and percentage of redirect requests

Bucket List

-   Bucket list information

    The Bucket list tab page displays the information including bucket name, region, creation time, metering statistics of the current month, and related operations.

    -   Display parameters are as follows: The metering statistics of the current month display the storage size, Internet outbound traffic, Put request count, and Get request count for each bucket.
    -   Click Monitoring chart or the corresponding bucket name to go to the bucket monitoring view page.
    -   Click Alarm rules next to your expected bucket, or go to the Alarm rules tab to display all alarm rules of the bucket.
    -   Enter the expected bucket name in the search box in the upper left-corner to display the bucket \(fuzzy match is supported\).
    -   Select the check boxes before the expected bucket names and click Setting custom monitor alarm rules to batch set alarm rules. For more information, see the [Alarm Service User Guide](intl.en-US/Developer Guide/Monitoring Services/Alarm service user guide.md#)
-   Bucket-level monitoring view

    Click **Monitoring chart** next to the expected bucket name in the bucket list to go to the bucket monitoring view.

    The bucket monitoring view displays metric charts based on the following six indicator groups:

    -   -   Monitoring service overview
-   Request status category
-   Measurement reference
-   Average latency
-   Maximum latency
-   Success request category
    Except measurement reference, other indicators are displayed with an aggregation granularity of 60s. The default time range for bucket-level metric charts is of the previous six hours, whereas that for user-level metric charts is of the previous hour. Click **Back to bucket list** in the upper-left corner to return to the Bucket list tab.

    -   Monitoring service overview

        This indicator group is similar to the service monitoring overview at the user level, but the former displays metric data at the bucket level. The main metric charts include:

        -   Request Valid Availability, which includes two metric indicators: availability and percentage of valid requests
        -   Total/Valid request, which includes two metric indicators: total number of requests and number of valid requests
        -   Overflow, which includes eight metric indicators: Internet outbound traffic, Internet inbound traffic, intranet outbound traffic, intranet inbound traffic, CDN outbound traffic, CDN inbound traffic, outbound traffic of cross-region replication, and inbound traffic of cross-region replication
        -   Request status count, which is a table that displays the number and percentage of each type of requests within the selected time range.
    -   Request status category

        This indicator group is similar to the request state details at the user level, but the former displays metric data at the bucket level. The main metric charts include:

        -   Server error count
        -   Server error rate
        -   Network error count
        -   Network error rate
        -   Client error request count, which includes four metric indicators: number of error requests indicating resource not found, number of authorization error requests, number of client-site time-out error requests, and number of other client-site error requests
        -   Client error request percent, which includes four metric indicators: percentage of error requests indicating resource not found, percentage of authorization error requests, percentage of client-site time-out error requests, and percentage of other client-site error requests
        -   Redirect request count, which includes two metric indicators: number of successful requests and number of redirect requests
        -   Success redirect rate, which includes two metric indicators: percentage of successful requests and percentage of redirect requests
        ![](images/4331_en-US.jpg)

    -   Measurement reference

        The metering reference group shows metering indicators with an hourly collection and representation granularity, as shown in the following figure:

        ![](images/1210_en-US.png)

        The metering metric charts include:

        -   Quota size
        -   Overflow
        -   Billing requests, which includes the Get request count and Put request count.
        After a bucket is created, new data is collected in the next hour on the hour following the current time point, and the collected data will be displayed within 30 minutes.

        ![](images/1211_en-US.png)

    -   Average latency

        This indicator group contains the average latency indicators of API monitoring. The metric charts include:

        -   getObject Average Latency
        -   headObject Average Latency
        -   putObject Average Latency
        -   postObject Average Latency
        -   append Object Average Latency
        -   upload Part Average Latency
        -   upload Part Copy Average Latency
        Each metric chart shows the corresponding average E2E latency and average server latency. See the following figure:

        ![](images/1212_en-US.png)

    -   Maximum latency

        This indicator group contains the maximum latency indicators of API monitoring. The metric charts include:

        -   getObject Max Latency\(Millisecond\)
        -   headObject Max Latency
        -   putObject Max Latency
        -   postObject Max Latency
        -   append Object Max Latency
        -   upload Part Max Latency
        -   upload Part Copy Max Latency
        Each metric chart shows the corresponding maximum E2E latency and maximum server latency. See the following figure:

        ![](images/1213_en-US.png)

    -   Success request category

        This indicator group contains the successful request count indicators of API monitoring. The metric charts include:

        -   getObject Success Count
        -   headObject Success Count
        -   putObject Success Count
        -   post Object Success Count
        -   append Object Success Count
        -   upload Part Success Count
        -   upload Part Copy Success Count
        -   delete Object Success Count
        -   deleteObjects Success Count
        See the following figure:

        ![](images/1214_en-US.png)


Alarm rules

The Alarm rules tab page allows you to view and manage all your alarm rules, as shown in the following figure:

![](images/1215_en-US.png)

For the description and usage of the "Alarm Rules" tab page, see the [Alarm Service User Guide](intl.en-US/Developer Guide/Monitoring Services/Alarm service user guide.md#).

## Additional links {#section_vpm_tvj_5db .section}

For more information regarding the important points and user guide of the monitoring service, see the related chapter in [Monitoring, diagnosis, and troubleshooting](intl.en-US/Developer Guide/Monitoring Services/Service monitoring, diagnosis, and troubleshooting.md#).

