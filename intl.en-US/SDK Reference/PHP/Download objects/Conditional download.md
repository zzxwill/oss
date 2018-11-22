# Conditional download {#concept_88498_zh .concept}

You can configure one or more conditions for downloads. If the specified conditions are met, the object can be downloaded. If the specified conditions are not met, an error code is returned and the object cannot not downloaded. You can configure the following conditions:

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|If-Modified-Since|If the specified time is earlier than the time the object is modified, the object can be downloaded. Otherwise, an error \(304 Not modified\) is returned.|OssClient::OSS\_IF\_MODIFIED\_SINCE|
|If-Unmodified-Since|If the specified time is later than or equal to the time the object is modified, the object can be downloaded. Otherwise, an error \(412 Precondition failed\) is returned.|OssClient::OSS\_IF\_UNMODIFIED\_SINCE|
|If-Match|If the specified ETag matches that of an object, the object can be downloaded. Otherwise, an error \(412 Precondition failed\) is returned.|OssClient::OSS\_IF\_MATCH|
|If-None-Match|If the specified ETag does not match that of an object, the object can be downloaded. Otherwise, an error \(304 Not modified\) is returned.|OssClient::OSS\_IF\_NONE\_MATCH|

**Note:** 

-   Both If-Modified-Since and If-Unmodified-Since can exist at the same time as object download conditions. Both If-Match and If-None-Match can exist at the same time as object download conditions.
-   $ossClient-\>getObjectMeta can be used to obtain the ETag.

You can download an OSS object to local memories or a local file in conditional downloads. Run the following code for conditional download:

```language-php
<? php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;

// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";
$object = "<yourObjectName>";

try{
    $options = array(
        OssClient::OSS_HEADERS => array(
            OssClient::OSS_IF_MODIFIED_SINCE => "Fri, 13 Nov 2015 14:47:53 GMT"),
    );

    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

    $content = $ossClient->getObject($bucket, $object, $options);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
print ($content);
print(__FUNCTION__ . ": OK" . "\n");

```

