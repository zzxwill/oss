# Image processing {#concept_47735_zh .concept}

Image Processing \(IMG\) is a massive, secure, cost-effective and highly reliable image processing service. After source images are uploaded to OSS, you can process images on any Internet device at any anytime, from anywhere through simple RESTful APIs.

For more information about IMG, see [Image Processing](../../../../reseller.en-US/Image Processing Guide/Image processing.md#). For the complete code of image processing, see [GitHub](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/samples/Image.php).

## Basic features {#section_brj_5wg_kfb .section}

OSS offers the following IMG features:

-   [Retrieve dominant image tones](../../../../reseller.en-US/Image Processing Guide/Obtain image information/Retrieve dominant image tones.md#)
-   [Format conversion](../../../../reseller.en-US/Image Processing Guide/Convert formats/Format conversion.md#)
-   [Resize images](../../../../reseller.en-US/Image Processing Guide/Resize images.md#)
-   [Incircle](../../../../reseller.en-US/Image Processing Guide/Crop images/Incircle.md#)
-   [Adaptive orientation](../../../../reseller.en-US/Image Processing Guide/Rotate images/Adaptive orientation.md#)
-   [Brightness](../../../../reseller.en-US/Image Processing Guide/Apply effects/Brightness.md#)
-   [Add watermarks](../../../../reseller.en-US/Image Processing Guide/Add watermarks.md#): allows you to add images, texts, or image-text watermarks to another image.
-   [Image processing](../../../../reseller.en-US/Image Processing Guide/Image processing.md#)
-   [Image processing access rules](../../../../reseller.en-US/Image Processing Guide/Image processing access rules.md#): calls multiple IMG functions.

## Usage { .section}

IMG uses standard HTTP GET. You can configure IMG parameters in QueryString of a URL.

If the ACL of an image object is private read and write, only authorized users are allowed for access.

-   Anonymous access

    You can use the following format in level-3 domains to access a processed image:

    ```
    http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction>,<yourParamValue>
    
    ```

    |Parameter|Description|
    |:--------|:----------|
    |bucket|Specifies the name of a bucket.|
    |endpoint|Specifies endpoint used to access a region.|
    |object|Specifies the name of an image object.|
    |image|Specifies the reserved identifier for IMG.|
    |action|Specifies the operations on an image, such as scaling, cropping, and rotating.|
    |param|Specifies the parameter that indicates the operation on an image.|

    -   Basic operations

        For example, scale an image to a width of 100 PX. Adjust the height based on the ratio.

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100
        
        ```

    -   Customized image styles

        You can use the following format in level-3 domains to access a processed image:

        ```
        http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=style/<yourStyleName>
        
        ```

        -   style: specifies a reserved identifier of a customized image style.
        -   yourStyleName: specifies the name of a custom image style. It is the name specified in the rule that is created on the OSS console.
        Example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/oss-pic-style-w-100
        
        ```

    -   Cascade operations

        Cascade operations allow you to perform multiple operations on an image sequentially.

        ```
        http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction1>,<yourParamValue1>/<yourAction2>,<yourParamValue2>/...
        
        ```

        Example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100/rotate,90
        
        ```

    -   HTTPS access

        IMG supports access through HTTPS. Example:

        ```
        https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100
        
        ```

-   Authorized access

    Authorized access allows you to customize an image style, HTTPS access, and cascade operations.

    Run the following code to generate a signed URL for IMG:

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
    
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
    
    // Generate a signed URL that is valid for 3600 seconds and can be directly accessed by browsers.
    $timeout = 3600;
    $options = array(
        OssClient::OSS_PROCESS => "image/resize,m_lfit,h_100,w_100" );
    $signedUrl = $ossClient->signUrl($bucket, $object, $timeout, "GET", $options);
    
    print("rtmp url: \n" . $signedUrl);
    
    ```

-   Access with SDK

    You can use SDK to access and process an image object.

    SDK allows you to customize image styles, HTTPS access, and cascade operations.

    -   Basic operations

        Run the following code to perform basic operations on an image:

        ```language-php
        <? php
        if (is_file(__DIR__ . '/../autoload.php')) {
            require_once __DIR__ . '/../autoload.php';
        }
        if (is_file(__DIR__ . '/../vendor/autoload.php')) {
            require_once __DIR__ . '/../vendor/autoload.php';
        }
        
        use OSS\OssClient;
        
        // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
        $accessKeyId = "<yourAccessKeyId>";
        $accessKeySecret = "<yourAccessKeySecret>";
        // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
        $endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        $bucket = "<yourBucketName>";
        $object = "<yourObjectName>";
        $download_file = "download.jpg";
        
        $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
        
        // Upload a sample image.
        $ossClient->uploadFile($bucket, $object, "example.jpg");
        
        // Scale the image.
        $options = array(
            OssClient::OSS_FILE_DOWNLOAD => $download_file,
            OssClient::OSS_PROCESS => "image/resize,m_fixed,h_100,w_100" );
        $ossClient->getObject($bucket, $object, $options);
        
        // Crop the image.
        $options = array(
            OssClient::OSS_FILE_DOWNLOAD => $download_file,
            OssClient::OSS_PROCESS => "image/crop,w_100,h_100,x_100,y_100,r_1" );
        $ossClient->getObject($bucket, $object, $options);
        
        // Rotate the image.
        $options = array(
            OssClient::OSS_FILE_DOWNLOAD => $download_file,
            OssClient::OSS_PROCESS => "image/rotate,90" );
        $ossClient->getObject($bucket, $object, $options);
        
        // Sharpen the image.
        $options = array(
            OssClient::OSS_FILE_DOWNLOAD => $download_file,
            OssClient::OSS_PROCESS => "image/sharpen,100" );
        $ossClient->getObject($bucket, $object, $options);
        
        // Add watermarks.
        $options = array(
            OssClient::OSS_FILE_DOWNLOAD => $download_file,
            OssClient::OSS_PROCESS => "image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ" );
        $ossClient->getObject($bucket, $object, $options);
        
        // Convert the image format.
        $options = array(
            OssClient::OSS_FILE_DOWNLOAD => $download_file,
            OssClient::OSS_PROCESS => "image/format,png" );
        $ossClient->getObject($bucket, $object, $options);
        
        // Obtain image information.
        $options = array(
            OssClient::OSS_FILE_DOWNLOAD => $download_file,
            OssClient::OSS_PROCESS => "image/info" );
        $ossClient->getObject($bucket, $object, $options);
        
        // Delete the sample image.
        $ossClient->deleteObject($bucket, $object);
        
        ```

    -   Customize an image style

        Run the following code to customize an image style:

        ```language-php
        <? php
        if (is_file(__DIR__ . '/../autoload.php')) {
            require_once __DIR__ . '/../autoload.php';
        }
        if (is_file(__DIR__ . '/../vendor/autoload.php')) {
            require_once __DIR__ . '/../vendor/autoload.php';
        }
        
        use OSS\OssClient;
        
        // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
        $accessKeyId = "<yourAccessKeyId>";
        $accessKeySecret = "<yourAccessKeySecret>";
        // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
        $endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        $bucket = "<yourBucketName>";
        $object = "<yourObjectName>";
        $download_file = "download.jpg";
        
        $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
        
        // Upload a sample image.
        $ossClient->uploadFile($bucket, $object, "example.jpg");
        
        // Customize an image style
        $style = "style/oss-pic-style-w-300";
        $options = array(
            OssClient::OSS_FILE_DOWNLOAD => $download_file,
            OssClient::OSS_PROCESS => $style);
        $ossClient->getObject($bucket, $object, $options);
        
        // Delete the sample image.
        $ossClient->deleteObject($bucket, $object);
        
        ```

    -   Cascade operations

        Use the following code to perform cascade operations on an image:

        ```language-php
        <? php
        if (is_file(__DIR__ . '/../autoload.php')) {
            require_once __DIR__ . '/../autoload.php';
        }
        if (is_file(__DIR__ . '/../vendor/autoload.php')) {
            require_once __DIR__ . '/../vendor/autoload.php';
        }
        
        use OSS\OssClient;
        
        // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
        $accessKeyId = "<yourAccessKeyId>";
        $accessKeySecret = "<yourAccessKeySecret>";
        // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
        $endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        $bucket = "<yourBucketName>";
        $object = "<yourObjectName>";
        $download_file = "download.jpg";
        
        $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);
        
        // Upload a sample image.
        $ossClient->uploadFile($bucket, $object, "example.jpg");
        
        // Perform cascade operations.
        $style = "image/resize,m_fixed,w_100,h_100/rotate,90";
        $options = array(
            OssClient::OSS_FILE_DOWNLOAD => $download_file,
            OssClient::OSS_PROCESS => $style);
        $ossClient->getObject($bucket, $object, $options);
        
        // Delete the sample image.
        $ossClient->deleteObject($bucket, $object);
        
        ```


## IMG tools { .section}

You can use the IMG viewer [ImageStyleViever](https://gosspublic.alicdn.com/image/index.html) to directly view the IMG result.

