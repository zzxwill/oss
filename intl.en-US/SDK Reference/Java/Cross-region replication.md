# Cross-region replication {#concept_32022_zh .concept}

Cross-region replication is used to automatically and asynchronously copy objects across buckets in different regions. Any changes \(creation, replacement, and deletion\) to objects in the source bucket will be synchronized to the target bucket. This function is used to meet the requirements of cross-region disaster recovery and data replication.

For more information about cross-region replication, see [Manage cross-region replication](../../../../reseller.en-US/Developer Guide/Manage files/Cross-region replication.md#).

## Enable cross-region replication {#section_v5b_bgd_kfb .section}

Run the following code to enable cross-region replication:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

AddBucketReplicationRequest request = new AddBucketReplicationRequest(bucketName);
request.setReplicationRuleID("<yourRuleId>");
request.setTargetBucketName("<yourTargetBucketName>");
// This example uses the target endpoint China (Beijing).
request.setTargetBucketLocation("oss-cn-beijing");
// Disable historical data replication. Historical data replication is enabled by default.
request.setEnableHistoricalObjectReplication(false);
ossClient.addBucketReplication(request);

// Close your OSSClient.
ossClient.shutdown();

```

## View cross-region replication configurations { .section}

Use the following code to view the cross-region replication configurations for a bucket:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

List<ReplicationRule> rules = ossClient.getBucketReplication(bucketName);
for (ReplicationRule rule : rules) {
    System.out.println(rule.getReplicationRuleID());
    System.out.println(rule.getTargetBucketLocation());
    System.out.println(rule.getTargetBucketName());
}

// Close your OSSClient.
ossClient.shutdown();

```

## View cross-region replication progress { .section}

Cross-region replication progress includes the progress of historical and real-time data synchronization.

-   Percent signs \(%\) indicate the progress of historical data synchronization. This function only works for buckets for which historical data synchronization has been enabled.
-   Time points \(the point of latest time the data is written\) indicate the progress of real-time data synchronization. If a time point is displayed, the data created before this time point has been synchronized.

Run the following code to view the cross-region replication progress:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

BucketReplicationProgress process = ossClient.getBucketReplicationProgress(bucketName, "<yourRuleId>");
System.out.println(process.getReplicationRuleID());
// Check whether historical data synchronization is enabled.
System.out.println(process.isEnableHistoricalObjectReplication());
// Print historical data synchronization progress.
System.out.println(process.getHistoricalObjectProgress());
// Print real-time data synchronization progress.
System.out.println(process.getNewObjectProgress());

// Close your OSSClient.
ossClient.shutdown();

```

## View the target regions for synchronization { .section}

Run the following code to obtain the target regions to which buckets can be synchronized:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

List<String> locations = ossClient.getBucketReplicationLocation(bucketName);
for (String loc : locations) {
    System.out.println(loc);
}

// Close your OSSClient.
ossClient.shutdown();

```

## Disable cross-region replication { .section}

Use the following code to disable cross-region replication:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Disable cross-region replication. After this function is disabled, objects in the target bucket exist and all changes in the source objects cannot be synchronized.
ossClient.deleteBucketReplication(bucketName, "<yourRuleId>");

// Close your OSSClient.
ossClient.shutdown();

```

