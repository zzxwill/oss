# Monitoring service overview {#concept_j5q_hlj_5db .concept}

The OSS monitoring service details metric data, including basic system operation statuses, performance, and metering. It also provides a custom alarm service to track requests, analyze usage, collect statistics on business trends, and promptly discover and diagnose system problems.

OSS metric indicators are classified into indicator groups such as basic service indicators, performance indicators, and metering indicators. For more information, see [Monitoring indicators reference](intl.en-US/Developer Guide/Monitoring Services/Monitoring indicators reference.md#).

## High real-time performance {#section_xhf_rlj_5db .section}

Real-time performance monitoring can expose potential peak/valley problems, display actual fluctuations, and provide insights into the analysis and evaluation of business scenarios. OSS real-time metric indicators \(excluding the metering indicator\) enable minute-level collection and aggregation of metric data with an output delay of less than one minute.

## Metering indicator description {#section_kfd_slj_5db .section}

The metering indicator uses the following features:

-   Metering entries are collected, aggregated, and output at the hour-level.
-   However, the output delay can be up to thirty minutes.
-   The time of metering refers to the start time of the relevant statistical period.
-   The metering acquisition cutoff time is the end time of the last metering data statistical period for the current month. If no metering data is produced during the current month, the metering data acquisition cutoff time is 00:00 on the first day of the current month.
-   A maximum amount of metering entries is pushed for presentation. For precise metering data, go to Billing Management and click [Usage Record](https://billing.console.aliyun.com/#/expense/outline).

For example, suppose that the user just uses the request to upload data, an average of 10 times a minute. So at 2016-05-10 08:00:00 to 2016-05-10 09:00:00 this hour of time, the measured data value of the user's number of put class requests is 600 times \(10\*60 minutes \), data time 2016-05-10 At 08:00:00, the data will be output at about 09:30:00. If this data is from 2016-05-01 From 00:00:00 to the present, the last measure monitoring data, then the cut-off time of the month for the measure data acquisition is 2016-05-10 09:00:00. If the user did not produce any measurement data in May 2016, the cut off time for the measurement collection is 2016-05-01 00:00:00.

## OSS alarm service {#section_sxg_tlj_5db .section}

You can set up to 1,000 alarm rules.

Alarm rules can be configured for other metric indicators, which can then be added to alarm monitoring. Additionally, multiple alarm rules may be configured for a single metric indicator.

-   For information about the alarm service, see [Alarm service overview](https://www.alibabacloud.com/help/doc-detail/28608.htm).
-   For instructions about how to use the OSS alarm service, see [Alarm service user guide](intl.en-US/Developer Guide/Monitoring Services/Alarm service user guide.md#).
-   For more information about OSS metric indicators, see [Monitoring indicators reference](intl.en-US/Developer Guide/Monitoring Services/Monitoring indicators reference.md#).

## Metric data retention policy {#section_dcf_5lj_5db .section}

Metric data is retained for 31 days and is automatically cleared upon expiration. To analyze metric data offline, or download and store historical metric data for longer than 31 days, use the appropriate tool or input code to read the data storage of CloudMonitor. For more information, see [Metric data access through the OpenAPI](#section_ywb_vlj_5db).

The console displays metric data up until the past seven days. To view historical metric data earlier than seven days, use the CloudMonitor SDK. For more information, see [Metric data access through the OpenAPI](#section_ywb_vlj_5db).

## Metric data access through the OpenAPI {#section_ywb_vlj_5db .section}

The OpenAPI of CloudMonitor allows you to access OSS metric data. For usage information, see:

-   [CloudMonitor OpenAPI User Manual](https://www.alibabacloud.com/help/doc-detail/28615.htm)
-   [Cloud monitoring SDK User Manual](https://www.alibabacloud.com/help/doc-detail/28621.htm)
-   [Metric item reference](intl.en-US/Developer Guide/Monitoring Services/Metric item reference .md#)

## Monitoring, diagnosis, and troubleshooting {#section_wpw_vlj_5db .section}

The following documentation provides monitoring, diagnosis, and troubleshooting details related to OSS management:

-   [Real-time service monitoring](intl.en-US/Developer Guide/Monitoring Services/Service monitoring, diagnosis, and troubleshooting.md#section_k51_nlk_5db) 

    Describes how to use the monitoring service to monitor the running status and performance of OSS.

-   [Tracking and diagnosis](intl.en-US/Developer Guide/Monitoring Services/Service monitoring, diagnosis, and troubleshooting.md#section_ixw_f4k_5db)

    Describes how to use the OSS monitoring service and logging function to diagnose problems, and how to associate relevant information in log files for tracking and diagnosis.

-   [Troubleshooting](intl.en-US/Developer Guide/Monitoring Services/Service monitoring, diagnosis, and troubleshooting.md#section_hmn_1pk_5db)

    Describes typical problems and corresponding troubleshooting methods.


## Considerations {#section_jv5_wlj_5db .section}

OSS buckets must be globally unique. If, after deleting a bucket, you create another bucket with the same name as the deleted bucket, the monitoring and alarm rules set for the deleted bucket are applied to the new bucket.

