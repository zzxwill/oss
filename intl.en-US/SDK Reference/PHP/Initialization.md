# Initialization {#concept_32100_zh .concept}

OSSClient serves as the OSS Java client to manage OSS resources such as buckets and objects.

## Create an OSSClient instance {#section_wsn_wq4_kfb .section}

To create an OSSClient instance, you need to specify an endpoint. For more information about endpoints, see [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Regions and endpoints.md#) and [Bind a custom domain name](../../../../reseller.en-US/Developer Guide/Access and control/Bind a custom domain name.md#).

-   Use an OSS domain to create an OSSClient instance

    Run the following code to create an OSSClient instance with a domain assigned by OSS:

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
    
    try {
    	$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
    } catch (OssException $e) {
    	print $e->getMessage();
    }
    
    ```

-   Use a custom domain \(CNAME\) to create an OSSClient instance

    Run the following code to create an OSSClient instance with CNAME:

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
    $endpoint = "<yourEndpoint>";
    
    try {
    	// The true parameter indicates that the CNAME is enabled. CNAME indicates a custom domain bound to a bucket.
    	$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, true);
    } catch (OssException $e) {
    	print $e->getMessage();
    }
    
    ```

    **Note:** The ossClient.listBuckets method is not available when CNAME is used.

-   Use STS to create an OSSClient instance

    Run the following code to create an OSSClient instance with Security Token Service \(STS\):

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
    $securityToken = "<yourSecurityToken>";
    
    try {
    	$ossClient = new OssClient(
            $accessKeyId, $accessKeySecret, $endpoint, false, $securityToken);
    } catch (OssException $e) {
    	print $e->getMessage();
    }
    
    ```

    For more information, see [What is RAM and STS](../../../../reseller.en-US/Best Practices/Access control/What is RAM and STS.md#) and [Authorized access](reseller.en-US/SDK Reference/PHP/Authorized access.md#).

-   Use a proxy server to create an OSSClient instance

    Run the following code to create an OSSClient instance with a proxy server, which is supported by PHP 5.3 and later.

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
    // The address of the proxy server, for example, http://<username>:<password>@<proxy ip>:<proxy port>
    $requestProxy = "<yourRequestProxy>";
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    $endpoint = "http://oss-cn-hangzhou.aliyuncs.com>";
    
    try {
        $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false, $requestProxy);
    } catch (OssException $e) {
        print $e->getMessage();
    }
    
    ```


## Configure network parameters { .section}

Run the following code to configure the network parameters of an OSSClient instance:

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

try {
    $ossClient = new OssClient(
        $accessKeyId, $accessKeySecret, $endpoint);
	// Set the timeout time in seconds for data transmission at the sockets layer. The default value is 5184000.
	$ossClient->setTimeout(3600);
	// Set the timeout time in seconds when connections are established. The default value is 10.
	$ossClient->setConnectTimeout(10);
} catch (OssException $e) {
    print $e->getMessage();
}

```

