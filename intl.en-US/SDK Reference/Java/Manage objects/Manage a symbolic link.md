# Manage a symbolic link {#concept_84848_zh .concept}

This topic describes how to manage a symbolic link.

## Create a symbolic link {#section_zmh_qcc_kfb .section}

A symbolic link is a special object that maps to an object similar to a shortcut used in Windows. You can configure user-defined Object Meta for symbolic links.

Run the following code to create a symbolic link:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String symLink = "<yourSymLink>";
String destinationObjectName = "<yourDestinationObjectName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Create Object Meta.
ObjectMetadata metadata = new ObjectMetadata();
metadata.setContentType("text/plain");
// Set property to property-value for user-defined metadata.
metadata.addUserMetadata("property", "property-value");

// Create CreateSymlinkRequest.
CreateSymlinkRequest createSymlinkRequest = new CreateSymlinkRequest(bucketName, symLink, destinationObjectName);

// Configure Object Meta.
createSymlinkRequest.setMetadata(metadata);

// Create symbolic links.
// Create a symbolic link.

// Close your OSSClient.
ossClient.shutdown();

```

For more information about symbolic links, see [PutSymlink](../../../../reseller.en-US/API Reference/Object operations/PutSymlink.md#).

## Obtain object content for a symbolic link { .section}

To obtain a symbolic link, you must have the read permission on it. Run the following code to obtain the object content for a symbolic link:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String symLink = "<yourSymLink>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Obtain a symbolic link.
OSSSymlink symbolicLink = ossClient.getSymlink(bucketName, symLink);
// Print the content of the object that the symbolic link directs to.
System.out.println(symbolicLink.getSymlink());
System.out.println(symbolicLink.getTarget());
System.out.println(symbolicLink.getRequestId());

// Close your OSSClient.
ossClient.shutdown();

```

For more information about symbolic links, see [GetSymlink](../../../../reseller.en-US/API Reference/Object operations/GetSymlink.md#).

