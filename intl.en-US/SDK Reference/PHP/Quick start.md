# Quick start {#concept_32101_zh .concept}

This topic describes how to use OSS PHP SDK to perform routine operations such as bucket creation, object uploads, and object downloads.

## Create a bucket {#section_n4g_s44_kfb .section}

Run the following code to create a bucket:

```language-php
<? php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;

// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Bucket name
$bucket = "<yourBucketName>";

try {
	$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
	$ossClient->createBucket($bucket);
} catch (OssException $e) {
	print $e->getMessage();
}

```

For more information about bucket naming rules, see naming conventions in [Basic concepts](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#). For more information about how to create a bucket, see [Manage a bucket](reseller.en-US/SDK Reference/PHP/Bucket.md#).

For more information about endpoints, see [Regions and endpoints](../../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).

## Upload objects { .section}

Run the following code to upload a file to OSS:

```language-php
<? php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;

// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Bucket name
$bucket= " <yourBucketName>";
// Object name
$object = " <yourObjectName>";
$content = "Hi, OSS.";

try {
	$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
	$ossClient->putObject($bucket, $object, $content);
} catch (OssException $e) {
	print $e->getMessage();
}

```

For more information, see [Upload objects](reseller.en-US/SDK Reference/PHP/Upload objects/Overview .md#).

## Download objects { .section}

Run the following code to download an object:

```language-php
<? php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;

// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Bucket name
$bucket= "<yourBucketName>";
// Object name
$object = "<yourObjectName>";

try {
	$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
	$content = $ossClient->getObject($bucket, $object);
    print("object content: " . $content);
} catch (OssException $e) {
	print $e->getMessage();
}

```

For more information, see [Download objects](reseller.en-US/SDK Reference/PHP/Download objects/Overview.md#).

## List objects { .section}

Run the following code to list objects in a specified bucket. You can list a maximum of 100 objects by default.

```language-php
<? php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;

// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Bucket name
$bucket= "<yourBucketName>";

try {
	$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
	
	$listObjectInfo = $ossClient->listObjects($bucket);
	$objectList = $listObjectInfo->getObjectList();
	if (! empty($objectList)) {
		foreach ($objectList as $objectInfo) {
		print($objectInfo->getKey() . "\t" . $objectInfo->getSize() . "\t" . $objectInfo->getLastModified() . "\n");
		}
	}
} catch (OssException $e) {
	print $e->getMessage();
}

```

For more information, see [List objects](reseller.en-US/SDK Reference/PHP/Manage objects/List objects.md#) in [Manage objects](reseller.en-US/SDK Reference/PHP/Manage objects/Overview.md#).

## Delete objects { .section}

Run the following code to delete a specified object:

```language-php
<? php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;

// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Bucket name
$bucket= "<yourBucketName>";
// Object name
$object = "<yourObjectName>";

try {
	$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
	$ossClient->deleteObject($bucket, $object);
} catch (OssException $e) {
	print $e->getMessage();
}

```

For more information, see [Delete objects](reseller.en-US/SDK Reference/PHP/Manage objects/Delete objects.md#) in [Manage objects](reseller.en-US/SDK Reference/PHP/Manage objects/Overview.md#).

