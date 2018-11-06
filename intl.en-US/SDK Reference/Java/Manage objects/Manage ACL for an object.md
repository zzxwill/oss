# Manage ACL for an object {#concept_84838_zh .concept}

This topic describes how to manage ACL for an object.

The following table describes the permissions included in the Access Control List \(ACL\) for an object.

|Permission|Description|Value|
|:---------|:----------|:----|
|default|The ACL of an object is the same with that of its bucket.|CannedAccessControlList.Default|
|Private|Only the object owner and authorized users can read and write the object.|CannedAccessControlList.Private|
|Public read|Only the object owner and authorized users can read and write the object. Other users can only read the object. Authorize this permission with caution.|CannedAccessControlList.PublicRead|
|Public read-write|All users can read and write the object. Authorize this permission with caution.|CannedAccessControlList.PublicReadWrite|

The ACL of objects take precedence over that of buckets. For example, if the ACL of a bucket is private, while the object ACL is public read-write, all users can read and write the object. If an object is not configured with an ACL, its ACL is the same as that of its bucket by default.

## Configure an ACL for an object {#section_ibx_xyb_kfb .section}

You can run the following code to configure an ACL for an object:

```language-java
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Configure the ACL for the object to public read.
ossClient.setObjectAcl("<yourBucketName>", "<yourObjectName>", CannedAccessControlList.PublicRead);

// Close your OSSClient.
ossClient.shutdown();

```

## Obtain the ACL for an object { .section}

You can run the following code to obtain the ACL for an object:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Obtain the ACL for an object.
ObjectAcl objectAcl = ossClient.getObjectAcl("<yourBucketName>", "<yourObjectName>");
System.out.println(objectAcl.getPermission().toString());

// Close your OSSClient.
ossClient.shutdown();

```

