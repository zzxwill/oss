# Metric item reference  {#concept_ij1_rfk_5db .concept}

This chapter provides parameter references to use with the OpenAPI, or the CloudMonitor SDK, to access the metric data of the OSS monitoring service.

**Project**

The OSS monitoring service metric data uses the same project name: acs\_oss.

Sample code written by the Java SDK:

```
QueryMetricRequest request = new QueryMetricRequest();
Request. setproject ("acs_oss ");
```

**StartTime and EndTime**

The value range of the time parameters for CloudMonitor is in the format of \[StartTime, EndTime\]. The data that is attributed to StartTime is not collected, whereas the data that is attributed to EndTime can be accessed.

The CloudMonitor retention policy specifies that data is retained for 31 days. This means the interval between StartTime and EndTime cannot exceed 31 days, and data outside the 31 day collection period cannot be accessed.

For more information about other time parameters, see [CloudMonitor API description](https://www.alibabacloud.com/help/doc-detail/28615.htm).

Sample code written by the Java SDK:

```
request.setStartTime("2016-05-15 08:00:00");
request.setEndTime("2015-05-15 09:00:00");
```

## Dimensions {#section_y1c_1gk_5db .section}

OSS metric items are classified into user level bucket level based on application scenarios. The value of Dimensions varies with regards to access of metric data at these different levels.

-   Dimensions does not need to be set for access to user-level metric data.
-   Set Dimensions access to bucket-level metric data as follows:

    ```
    {"BucketName": "your_bucket_name"}
    ```

    your\_bucket\_name indicates the name of the bucket you want to access.

    Note: Dimensions is a JSON string and has only one Key-Value pair for OSS metric indicators.

    Sample code written by the Java SDK:

    ```
    request.setDimensions("{\"BucketName\":\"your_bucket_name\"}");
    ```


## Period {#section_gw4_fgk_5db .section}

The aggregation granularity of all OSS metric indicators, except metering indicators, is 60s by default. The aggregation granularity of metering indicators is 3,600s by default.

Sample code written by the Java SDK:

```
request.setPeriod("60");
```

## Metric {#section_csl_zgk_5db .section}

The [Monitoring indicators reference](intl.en-US/Developer Guide/Monitoring Services/Monitoring indicators reference.md#) describes the following metric items.

|Metric|Metric item name|Unit|Level|
|:-----|:---------------|:---|:----|
|Useravailability|User-level availability|%|User level|
|UserRequestValidRate|User-level valid request rate|%|User level|
|UserTotalRequestCount|User-level requests|Times|User level|
|UserValidRequestCount|User-level valid requests|Times|User level|
|UserInternetSend|User-level Internet outbound traffic|Byte|User level|
|UserInternetRecv|User-level Internet inbound traffic|Byte|User level|
|UserIntranetSend|User-level intranet outbound traffic|Byte|User level|
|UserIntranetRecv|User-level intranet inbound traffic|Byte|User level|
|UserCdnSend|User-level CDN outbound traffic|Byte|User level|
|UserCdnRecv|User-level CDN inbound traffic|Byte|User level|
|UserSyncSend|User-level outbound traffic of cross-region replication|Byte|User level|
|UserSyncRecv|User-level inbound traffic of cross-region replication|Byte|User level|
|UserServerErrorCount|User-level server-site error requests|Times|User level|
|UserServerErrorRate|User-level server-site error request rate|%|User level|
|UserNetworkErrorCount|User-level network-site error requests|Times|User level|
|UserNetworkErrorRate|User-level network-site error request rate|%|User level|
|UserAuthorizationErrorCount|User-level client-site authorization error requests|Times|User level|
|UserAuthorizationErrorRate|User-level client-site authorization error request rate|%|User level|
|UserResourceNotFoundErrorCount|User-level client-site error requests indicating resource not found|Times|User level|
|UserResourceNotFoundErrorRate|User-level client-site error request rate indicating resource not found|%|User level|
|UserClientTimeoutErrorCount|User-level client-site time-out error request|Times|User level|
|UserClientOtherErrorRate|User-level client-site time-out error request rate|%|User level|
|UserClientOtherErrorCount|Other user-level client-site error requests|Times|User level|
|UserClientOtherErrorRate|Other user-level client-site error request rate|%|User level|
|UserSuccessCount|Successful user-level requests|Times|User level|
|UserSuccessRate|Successful user-level request rate|%|User level|
|UserRedirectCount|User-level redirect requests|Times|User level|
|UserRedirectRate|User-level redirect request rate|%|User level|
|Availability|Availability|%|Bucket level|
|RequestValidRate|Valid request rate|%|Bucket level|
|TotalRequestCount|Requests|Times|Bucket level|
|ValidRequestCount|Valid requests|Times|Bucket level|
|InternetSend|Internet outbound traffic|Byte|Bucket level|
|InternetRecv|Internet inbound traffic|Byte|Bucket level|
|IntranetSend|Intranet outbound traffic|Byte|Bucket level|
|IntranetRecv|Intranet inbound traffic|Byte|Bucket level|
|CdnSend|CDN outbound traffic|Byte|Bucket level|
|CdnRecv|CDN inbound traffic|Byte|Bucket level|
|SyncSend|Outbound traffic of cross-region replication|Byte|Bucket level|
|SyncRecv|Inbound traffic of cross-region replication|Byte|Bucket level|
|ServerErrorCount|Server-site error requests|Times|Bucket level|
|ServerErrorRate|Server-site error request rate|%|Bucket level|
|NetworkErrorCount|Network-site error requests|Times|Bucket level|
|NetworkErrorRate|Network-site error request rate|%|Bucket level|
|AuthorizationErrorCount|Client-site authorization error requests|Times|Bucket level|
|AuthorizationErrorRate|Client-site authorization error request rate|%|Bucket level|
|ResourceNotFoundErrorCount|Client-site error requests indicating resource not found|Times|Bucket level|
|ResourceNotFoundErrorRate|Client-site error request rate indicating resource not found|%|Bucket level|
|ClientTimeoutErrorCount|Client-site time-out error requests|Times|Bucket level|
|ClientOtherErrorRate|Client-site time-out error request rate|%|Bucket level|
|ClientOtherErrorCount|Other client-site error requests|Times|Bucket level|
|ClientOtherErrorRate|Other client-site error request rate|%|Bucket level|
|SuccessCount|Successful requests|Times|Bucket level|
|SuccessRate|Successful request rate|%|Bucket level|
|RedirectCount|Redirect requests|Times|Bucket level|
|RedirectRate|Redirect request rate|%|Bucket level|
|GetObjectE2eLatency|Average E2E latency of GetObject requests|Millisecond|Bucket level|
|GetObjectServerLatency|Average server latency of GetObject requests|Millisecond|Bucket level|
|MaxGetObjectE2eLatency|Maximum E2E latency of GetObject requests|Millisecond|Bucket level|
|MaxGetObjectServerLatency|Maximum server latency of GetObject requests|Millisecond|Bucket level|
|HeadObjectE2eLatency|Average E2E latency of HeadObject requests|Millisecond|Bucket level|
|HeadObjectServerLatency|Average server latency of HeadObject requests|Millisecond|Bucket level|
|MaxHeadObjectE2eLatency|Maximum E2E latency of HeadObject requests|Millisecond|Bucket level|
|MaxHeadObjectServerLatency|Maximum server latency of HeadObject requests|Millisecond|Bucket level|
|PutObjectE2eLatency|Average E2E latency of PutObject requests|Millisecond|Bucket level|
|PutObjectServerLatency|Average server latency of PutObject requests|Millisecond|Bucket level|
|MaxPutObjectE2eLatency|Maximum E2E latency of PutObject requests|Millisecond|Bucket level|
|MaxPutObjectServerLatency|Maximum server latency of PutObject requests|Millisecond|Bucket level|
|PostObjectE2eLatency|Average E2E latency of PostObject requests|Millisecond|Bucket level|
|PostObjectServerLatency|Average server latency of PostObject requests|Millisecond|Bucket level|
|MaxPostObjectE2eLatency|Maximum E2E latency of PostObject requests|Millisecond|Bucket level|
|MaxPostObjectServerLatency|Maximum server latency of PostObject requests|Millisecond|Bucket level|
|AppendObjectE2eLatency|Average E2E latency of AppendObject requests|Millisecond|Bucket level|
|AppendObjectServerLatency|Average server latency of AppendObject requests|Millisecond|Bucket level|
|MaxAppendObjectE2eLatency|Maximum E2E latency of AppendObject requests|Millisecond|Bucket level|
|MaxAppendObjectServerLatency|Maximum server latency of AppendObject requests|Millisecond|Bucket level|
|UploadPartE2eLatency|Average E2E latency of UploadPart requests|Millisecond|Bucket level|
|UploadPartServerLatency|Average server latency of UploadPart requests|Millisecond|Bucket level|
|MaxUploadPartE2eLatency|Maximum E2E latency of UploadPart requests|Millisecond|Bucket level|
|MaxUploadPartServerLatency|Maximum server latency of UploadPart requests|Millisecond|Bucket level|
|UploadPartCopyE2eLatency|Average E2E latency of UploadPartCopy requests|Millisecond|Bucket level|
|UploadPartCopyServerLatency|Average server latency of UploadPartCopy requests|Millisecond|Bucket level|
|MaxUploadPartCopyE2eLatency|Maximum E2E latency of UploadPartCopy requests|Millisecond|Bucket level|
|MaxUploadPartCopyServerLatency|Maximum server latency of UploadPartCopy requests|Millisecond|Bucket level|
|GetObjectCount|Successful GetObject requests|Times|Bucket level|
|HeadObjectCount|Successful HeadObject requests|Times|Bucket level|
|PutObjectCount|Successful PutObject requests|Times|Bucket level|
|PostObjectCount|Successful PostObject requests|Times|Bucket level|
|AppendObjectCount|Successful AppendObject requests|Times|Bucket level|
|UploadPartCount|Successful UploadPart requests|Times|Bucket level|
|UploadPartCopyCount|Successful UploadPartCopy requests|Times|Bucket level|
|DeleteObjectCount|Successful DeleteObject requests|Times|Bucket level|
|DeleteObjectsCount|Successful DeleteObjects requests|Times|Bucket level|

The following table lists the metric items of metering indicators with an aggregation granularity of 3,600s.

|Metric|Metric item name|Unit|Level|
|:-----|:---------------|:---|:----|
|MeteringStorageUtilization|Size of storage|Byte|If Dimensions is set, the returned metric data belongs to the bucket level; if Dimensions is not set, the returned metric data belongs to the user level.|
|MeteringGetRequest|Get requests|Times|If Dimensions is set, the returned metric data belongs to the bucket level; if Dimensions is not set, the returned metric data belongs to the user level.|
|MeteringPutRequest|Put requests|Times|If Dimensions is set, the returned metric data belongs to the bucket level; if Dimensions is not set, the returned metric data belongs to the user level.|
|Meteringinternettx|Volume of Internet outbound traffic|Byte|If Dimensions is set, the returned metric data belongs to the bucket level; if Dimensions is not set, the returned metric data belongs to the user level.|
|MeteringCdnTX|Volume of CDN outbound traffic|Byte|If Dimensions is set, the returned metric data belongs to the bucket level; if Dimensions is not set, the returned metric data belongs to the user level.|
|MeteringSyncRX|Volume of inbound traffic of cross-region replication|Byte|If Dimensions is set, the returned metric data belongs to the bucket level; if Dimensions is not set, the returned metric data belongs to the user level.|

Sample code written by the Java SDK:

```
request.setMetric("UserAvailability");
```

