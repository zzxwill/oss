# Anti-leech {#concept_32021_zh .concept}

This topic describes how to use anti-leech.

To prevent your data on OSS from being leeched, OSS supports anti-leeching through the referer field settings in the HTTP header, including the following parameters:

-   Referer whitelist: Used to allow access only for specified domains to OSS data.
-   Empty referer: Determines whether the referer can be empty. If it is not allowed, only requests with the referer filed in their HTTP or HTTPS headers can access OSS data.

For more information about anti-leaching, see [Anti-leeching settings](../../../../reseller.en-US/Developer Guide/Security management/Anti-leech settings.md#).

## Configure the referer whitelist {#section_ydj_tfd_kfb .section}

Run the following code to configure the referer whitelist:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

List<String> refererList = new ArrayList<String>();
// Add the referer field. The referer field allows question marks (?) and asterisks (*) for wildcard use.
refererList.add("http://www.aliyun.com");
refererList.add("http://www. *.com");
refererList.add("http://www.?.aliyuncs.com");
// Configure the referer list for a bucket. Set the parameter to true, which allows the referer field to be empty.
BucketReferer br = new BucketReferer(true, refererList);
ossClient.setBucketReferer(bucketName, br);

// Close your OSSClient.
ossClient.shutdown();

```

## Obtain a referer whiltelist { .section}

Run the following code to obtain a referer whiltelist:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Obtain the referer list for a bucket.
BucketReferer br = ossClient.getBucketReferer(bucketName);
List<String> refererList = br.getRefererList();
for (String referer : refererList) {
	System.out.println(referer);
}

// Close your OSSClient.
ossClient.shutdown();

```

## Clear a referer whitelist { .section}

Run the following code to clear a referer whitelist:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// You cannot clear a referer whitelist directly. To clear a referer whitelist, you need to create the rule that allows an empty referer field and replace the original rule with the new rule.
BucketReferer br = new BucketReferer();
ossClient.setBucketReferer(bucketName, br);

// Close your OSSClient.
ossClient.shutdown();

```

