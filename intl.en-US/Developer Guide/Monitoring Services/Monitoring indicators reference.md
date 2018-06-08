# Monitoring indicators reference {#concept_ux3_yhk_5db .concept}

OSS indicators can be monitored at the user level or the bucket level based on application scenarios.

In addition to common chronological metric indicators, the system analyzes and collects statistics on the existing metric indicators for easy user observation of metric data and matching of billing policy. Statistical indicators over a specified period of time are provided, such as request status distribution and metering statistics of the month. This reference guide describes the indicators in detail.

All indicators are collected at the minute-level \(per minute\) except for metering and statistical indicators. Metering indicators are collected at the hour-level \(per hour\).

## User-level indicators {#section_enz_zhk_5db .section}

The user level indicator refers to the indicator information that monitors the overall situation of the OSS system used from the user's account level, and is a summary of all bucket related monitoring data under the account. User-level indicators consist of three monitoring indicator details: current-month metering statistics, service monitoring overview, and request state details.

Service monitoring overview

Indicators in service monitoring overview are basic service indicators. Details of service monitoring overview indicators are as follows:

|Indicator|Unit|Description|
|:--------|:---|:----------|
|Availability|%|An indicator showing the system availability of using the storage service. It is obtained through the equation:  `1 - percentage of requests with server-end errors (indicated by a return code 5xx) in all requests`.|
|Valid requests rate|%| Percentage of valid requests in all requests. For more information about valid requests, see the following description.|
|Requests|Times|Total number of requests received and processed by the OSS server|
|Valid requests|Times|Total number of requests whose return code is 2xx or 3xx.|
|Internet outbound traffic|Byte|Downstream Internet traffic|
|Internet inbound traffic|Byte|Upstream Internet traffic|
|Intranet outbound traffic|Byte|Downstream intranet traffic of the service system|
|Intranet inbound traffic|Byte|Upstream intranet traffic of the service system|
|CDN outbound traffic|Byte|Downstream CDN traffic when CDN acceleration service is activated, that is, the origin retrieval traffic|
|CDN inbound traffic|Byte|Upstream CDN traffic when CDN acceleration service is activated|
|Outbound traffic of cross-region replication|Byte|Downstream traffic generated in the data replication process when the cross-region replication function is activated|
|Inbound traffic of cross-region replication|Byte|Upstream traffic generated in the data replication process when the cross-region replication function is activated|

In addition to the above specific monitoring indicators, statistics are also provided for the distribution of request status over a period of time. The statistics are mainly based on the status codes of the returned status codes or OSS error codes \(the total number and the percentage of requests within the observed time period\). 

Request state details

Request state details indicators are requested monitoring information based on the return status code, or OSS error code, associated with the different requests.  Details of request state details indicators are as follows:

|Indicator|Unit|Description|
|:--------|:---|:----------|
|Server-site error requests|Times|Total number of requests with system-level errors indicated by a return code 5xx|
|Server-site error requests rate|%|Percentage of requests with server-end errors in all requests|
|Network error requests|Times|Total number of requests whose [HTTP status code is 499](https://httpstatuses.com/499)|
|Network error requests rate|%|Percentage of requests with network errors in all requests|
|Client-end authorization error requests|Times|Total number of requests with a return code 403|
|Client-end authorization error requests rate|%|Percentage of requests with client-end authorization errors in all requests|
|Client-end error requests indicating resource not found|Times|Total number of requests with a return code 404|
|Client-end error requests rate indicating resource not found|%|Percentage of requests with client-end errors indicating resource not found in all requests|
|Client-end time-out error requests|Times| Total number of requests whose return status code is 408 or return OSS error code is RequestTimeout|
|Client-end time-out error requests rate|%|Percentage of requests with client-end time-out errors in all requests|
|Other client-end error requests|Times|Total number of requests with other client-end errors indicated by a return code 4xx|
|Other client-end error requests rate|%|Percentage of requests with other client-end errors in all requests|
|Successful requests|Times|Total number of requests whose return code is 2xx.|
|Successful requests rate|%|Percentage of successful requests in all requests|
|Redirect requests|Times|Total number of requests whose return code is 3xx.|
|Redirect requests rate|%|Percentage of redirect requests in all requests|

Current-month metering statistics

Metering statistics of the current month are collected from 00:00 on the first day of the month to the metering cutoff time as indicated in the same month.

Details of the metering indicators currently available are as follows:

|Indicator|Unit|Description|
|:--------|:---|:----------|
|Storage size|Byte|Size of the total storage occupied by all buckets of a specified user before the metering statistic collection deadline.|
|Internet outbound traffic|Byte|Total Internet outbound traffic of the user from 00:00 of the first day of the current month to the metering statistic collection deadline.|
|Put requests|Times|Total number of Put requests of the user from 00:00 of the first day of the current month to the metering statistic collection deadline.|
|Get requests|Times|Total number of Get requests of the user from 00:00 of the first day of the current month to the metering statistic collection deadline.|

## Bucket-level indicators {#section_fbb_k3k_5db .section}

Bucket-level indicators are used to monitor OSS operations of specific buckets and are applicable for business scenarios. In addition to current-month metering statistics and basic service indicator items such as service monitoring overview and request state details \(which can be monitored at the account level\), bucket-level indicators include metering indicators and performance indicators such as metering reference, latency, and successful request operation categories.

Service monitoring overview

Similar to the user-level description, the service monitoring overview indicators are basic indicators, but use metric data that is displayed at the bucket-level.

Request state details

Similar to the user-level description, the request state details indicators use metric data that is displayed at the bucket-level.

Current-month metering statistics

Statistical methods are similar to those listed in current-month metering statistics at the user level, but the former collects resource usage statistics at the bucket level. Details of current-month metering statistics at the bucket-level are as follows:

|Indicator|Unit|Description|
|:--------|:---|:----------|
|Storage size|Byte|Size of storage occupied by a specified bucket before the metering statistic collection deadline.|
|Internet outbound traffic|Byte|Total Internet outbound traffic of a specified bucket from 00:00 of the first day of the current month to the metering statistic collection deadline.|
|Put requests| Times|Total number of Put requests of a specified bucket from 00:00 of the first day of the current month to the metering statistic collection deadline.|
|Get requests|Times|Total number of Get requests of a specified bucket from 00:00 of the first day of the current month to the metering statistic collection deadline.|

Metering indicators

Metering indicators are monitored chronologically, and are collected and aggregated at the hour-level. Details of metering indicators are as follows:

|Indicator|Unit|Description|
|:--------|:---|:----------|
|Storage size|Byte|Average size of storage used by a specified bucket in an hour.|
|Internet outbound traffic|Byte|Total Internet outbound traffic of a specified bucket in an hour.|
|Put requests|Times|Total number of Put requests of a specified bucket in an hour.|
|Get requests|Times|Total number of Gut requests of a specified bucket in an hour.|

Latency

Latency Request latency directly reflects the system performance and is monitored using two indicators: average latency and maximum latency. The indicators are collected and aggregated at the minute-level. Moreover, indicators can be classified based on the OSS API request operation type to more specifically reflect the performance of the system responding to different operations. Only APIs involving data operations in bucket-related operations \(excluding meta operations\) are monitored currently.

Besides, in order to facilitate analyzing performance hotspots and environmental problems, latency monitoring indicators are collected from two different links of E2E and the server, in which:

-   E2E latency refers to the E2E latency of sending a successful request to OSS, including the processing time OSS requires to read the request, send a response, and receive a response confirmation message.
-   Server latency is the latency of OSS processing a successful request, excluding the network delay involved in E2E latency.

Note that performance indicators are used to monitor successful requests \(with a return status code 2xx\).

The following table lists specific metric indicator items:

|Indicator|Unit|Description|
|:--------|:---|:----------|
|Average E2E latency of GetObject requests|Millisecond|Average E2E latency of successful requests whose request API is GetObject|
|Average server latency of GetObject requests|Millisecond|Average server latency of successful requests whose request API is GetObject|
|Maximum E2E latency of GetObject requests|Millisecond|Maximum E2E latency of successful requests whose request API is GetObject|
|Maximum server latency of GetObject requests|Millisecond|Maximum server latency of successful requests whose request API is GetObject|
|Average E2E latency of HeadObject requests|Millisecond|Average E2E latency of successful requests whose request API is HeadObject|
|Average server latency of HeadObject requests|Millisecond| Average server latency of successful requests whose request API is HeadObject|
|Maximum E2E latency of HeadObject requests|Millisecond|Maximum E2E latency of successful requests whose request API is HeadObject|
|Maximum server latency of HeadObject requests|Millisecond|Maximum server latency of successful requests whose request API is HeadObject|
|Average E2E latency of PutObject requests|Millisecond|Average E2E latency of successful requests whose request API is PutObject|
|Average server latency of PutObject requests|Millisecond|Average server latency of successful requests whose request API is PutObject|
|Maximum E2E latency of PutObject requests|Millisecond|Maximum E2E latency of successful requests whose request API is PutObject|
|Maximum server latency of PutObject requests|Millisecond|Maximum server latency of successful requests whose request API is PutObject|
|Average E2E latency of PostObject requests|Millisecond|Average E2E latency of successful requests whose request API is PostObject|
|Average server latency of PostObject requests|Millisecond|Average server latency of successful requests whose request API is PostObject|
|Maximum E2E latency of PostObject requests|Millisecond| Maximum E2E latency of successful requests whose request API is PostObject|
|Maximum server latency of PostObject requests|Millisecond|Maximum server latency of successful requests whose request API is PostObject|
|Average E2E latency of AppendObject requests|Millisecond|Average E2E latency of successful requests whose request API is AppendObject|
|Average server latency of AppendObject requests|Millisecond|Average server latency of successful requests whose request API is AppendObject|
|Maximum E2E latency of AppendObject requests|Millisecond|Maximum E2E latency of successful requests whose request API is AppendObject|
|Maximum server latency of AppendObject requests|Millisecond|Maximum server latency of successful requests whose request API is AppendObject|
|Average E2E latency of UploadPart requests|Millisecond|Average E2E latency of successful requests whose request API is UploadPart|
|Average server latency of UploadPart requests|Millisecond|Average server latency of successful requests whose request API is UploadPart|
|Maximum E2E latency of UploadPart requests|Millisecond|Maximum E2E latency of successful requests whose request API is UploadPart|
|Maximum server latency of UploadPart requests|Millisecond|Maximum server latency of successful requests whose request API is UploadPart|
|Average E2E latency of UploadPartCopy requests|Millisecond|Average E2E latency of successful requests whose request API is UploadPartCopy|
|Average server latency of UploadPartCopy requests|Millisecond|Average server latency of successful requests whose request API is UploadPartCopy|
|Maximum E2E latency of UploadPartCopy requests|Millisecond|Maximum E2E latency of successful requests whose request API is UploadPartCopy|
|Maximum server latency of UploadPartCopy requests|Millisecond|Maximum server latency of successful requests whose request API is UploadPartCopy|

Successful request operation categories

In conjunction with latency monitoring, the monitoring of successful requests reflects the system capability of processing access requests to a certain extent.  Similarly, only APIs involving data operations in bucket-related operations are monitored currently. The following lists specific indicator items:

|Indicator|Unit|Description|
|:--------|:---|:----------|
|Successful GetObject requests|Times|Number of successful requests whose request API is GetObject|
|Successful HeadObject requests|Times|Number of successful requests whose request API is HeadObject|
|Successful PutObject requests|Times|Number of successful requests whose request API is PutObject|
|Successful PostObject requests|Times|Number of successful requests whose request API is PostObject|
|Successful AppendObject requests|Times|Number of successful requests whose request API is AppendObject|
|Successful UploadPart requests| Times|Number of successful requests whose request API is UploadPart|
|Successful UploadPartCopy requests|Times|Number of successful requests whose request API is UploadPartCopy|
|Successful DeleteObject requests|Times|Number of successful requests whose request API is DeleteObject|
|Successful DeleteObjects requests|Times|Number of successful requests whose request API is DeleteObjects|

