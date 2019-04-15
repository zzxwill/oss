# IMG {#concept_mh5_xth_vgb .concept}

Image Processing \(IMG\) is a secure, cost-effective, and highly reliable image processing service provided by OSS. IMG is capable of processing large amounts of images. After source images are uploaded to OSS, you can use Representational State Transfer \(RESTful\) APIs to process images anytime, anywhere, and on any Internet device.

For more information about IMG, see [OSS Image Processing Guide](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#).

## Basic features {#section_brj_5wg_kfb .section}

IMG has the following features:

-   [Obtain image information](../../../../../reseller.en-US/Data Processing/Image Processing/Obtain image information/Retrieve dominant image tones.md#)
-   [Convert formats](../../../../../reseller.en-US/Data Processing/Image Processing/Convert formats/Format conversion.md#)
-   [Resize images](../../../../../reseller.en-US/Data Processing/Image Processing/Resize images.md#)
-   [Crop images](../../../../../reseller.en-US/Data Processing/Image Processing/Crop images/Incircle.md#)
-   [Rotate images](../../../../../reseller.en-US/Data Processing/Image Processing/Rotate images/Adaptive orientation.md#)
-   [Apply effects](../../../../../reseller.en-US/Data Processing/Image Processing/Apply effects/Brightness.md#)
-   [Add watermarks](../../../../../reseller.en-US/Data Processing/Image Processing/Add watermarks.md#): allows you to add image, text, or image-text watermarks to another image.
-   [Customize image styles](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#)
-   [Cascade operations](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing access rules.md#): calls multiple IMG operations.

## How to use IMG {#section_gc5_swg_kfb .section}

IMG uses the standard HTTP GET request. You can configure IMG parameters in QueryString of a URL.

If the ACL of an image object is Private, only authorized users are allowed access.

-   Anonymous access

    You can use level-3 domains to access processed images. The access format is as follows:

    ```
    http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction>,<yourParamValue>
    ```

    |Name|Description|
    |:---|:----------|
    |bucket|The name of the bucket.|
    |endpoint|The endpoint used to access the region to which the bucket belongs.|
    |object|The name of the image object.|
    |image|The reserved identifier for IMG.|
    |action|Operations on the image, such as resizing, cropping, and rotating.|
    |param|The parameter corresponding to the operations performed on the image.|

    -   Basic operations

        For example, scale an image to a width of 100 px. Adjust the height based on the ratio.

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100
        ```

    -   Customize an image style

        You can use level-3 domains to access processed images. The access format is as follows:

        ```
        http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=style/<yourStyleName>
        ```

        -   style: specifies the reserved identifier of a custom image style.
        -   yourStyleName: specifies the name of a custom image style. It is the name of the rule that is specified in the OSS console.
        Example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/oss-pic-style-w-100
        ```

    -   Cascade operations

        Cascade operations allow you to perform multiple operations on an image in sequence. The format is as follows:

        ```
        http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction1>,<yourParamValue1>/<yourAction2>,<yourParamValue2>/...
        ```

        Example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100/rotate,90
        ```

    -   Access through HTTPS

        IMG supports access through HTTPS. Example:

        ```
        https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100
        ```

-   Authorized access

    Authorized access allows you to access an image through HTTPS, customize the image style, and perform cascade operations.

    Use the following code to generate a signed URL for IMG:

    ```
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
          /* Initialize the OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string BucketName = "yourBucketName";
        std::string ObjectName = "yourObjectName";
    
          /* Initialize network resources. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
     
       
        /* Generate the signed URL. */
        std::string Process = "image/resize,m_fixed,w_100,h_100";
        GeneratePresignedUrlRequest request(BucketName, ObjectName, Http::Get);
        request.setProcess(Process);
        auto outcome = client.GeneratePresignedUrl(request);
    
        /* Release network resources. */
        ShutdownSdk();
        return 0;
    }
    ```

-   Access with an SDK

    You can use an SDK to access and process an image object.

    The SDK allows you to access an image through HTTPS, customize the image style, and perform cascade operations.

    -   Basic operations

        Use the following code to perform basic operations on an image:

        ```
        #include <alibabacloud/oss/OssClient.h>
        using namespace AlibabaCloud::OSS;
        
        int main(void)
        {
             /* Initialize the OSS account information. */
            std::string AccessKeyId = "yourAccessKeyId";
            std::string AccessKeySecret = "yourAccessKeySecret";
            std::string Endpoint = "yourEndpoint";
            std::string BucketName = "yourBucketName";
            std::string ObjectName = "yourObjectName";
        
             /* Initialize network resources. */
            InitializeSdk();
        
            ClientConfiguration conf;
            OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
          
            /* Scale the image. */
            std::string Process = "image/resize,m_fixed,w_100,h_100";
            GetObjectRequest request(BucketName, ObjectName);
            request.setProcess(Process);
            auto outcome = client.GetObject(request);
          
            /* Crop the image. */
            Process = "image/crop,w_100,h_100,x_100,y_100,r_1";
            request.setProcess(Process);
            outcome = client.GetObject(request);
          
            /* Rotate the image. */
            Process = "image/rotate,90";
            request.setProcess(Process);
            outcome = client.GetObject(request);
          
            /* Sharpen the image. */
            Process = "image/sharpen,100";
            request.setProcess(Process);
            outcome = client.GetObject(request);
          
            /* Add the watermark. */
            Process = "image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ";
            request.setProcess(Process);
            outcome = client.GetObject(request);
          
            /* Convert the image format. */
            Process = "image/format,png";
            request.setProcess(Process);
            outcome = client.GetObject(request);
          
            /* Obtain image information. */
            request = request(BucketName, ObjectName, "image/info") ;
            outcome = client.GetObject(request);
        
            /* Release network resources. */
            ShutdownSdk();
            return 0;
        }
        ```

    -   Customize an image style

        Use the following code to customize an image style:

        ```
        #include <alibabacloud/oss/OssClient.h>
        using namespace AlibabaCloud::OSS;
        
        int main(void)
        {
             /* Initialize the OSS account information. */
            std::string AccessKeyId = "yourAccessKeyId";
            std::string AccessKeySecret = "yourAccessKeySecret";
            std::string Endpoint = "yourEndpoint";
            std::string BucketName = "yourBucketName";
            std::string ObjectName = "yourObjectName";
        
             /* Initialize network resources. */
            InitializeSdk();
        
            ClientConfiguration conf;
            OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf) ;
          
            /* Customize the image style. */
            std::string Process = "style/<yourCustomStyleName";
            GetObjectRequest request(BucketName, ObjectName);
            request.setProcess(Process);
            auto outcome = client.GetObject(request);
        
            /* Release network resources. */
            ShutdownSdk();
            return 0;
        }
        ```

    -   Cascade operations

        Use the following code to perform cascade operations on an image:

        ```
        #include <alibabacloud/oss/OssClient.h>
        using namespace AlibabaCloud::OSS;
        
        int main(void)
        {
             /* Initialize the OSS account information. */
            std::string AccessKeyId = "yourAccessKeyId";
            std::string AccessKeySecret = "yourAccessKeySecret";
            std::string Endpoint = "yourEndpoint";
            std::string BucketName = "yourBucketName";
            std::string ObjectName = "yourObjectName";
        
             /* Initialize network resources. */
            InitializeSdk();
        
            ClientConfiguration conf;
            OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
          
            /* Perform cascade operations. */
            std::string Process = "image/resize,m_fixed,w_100,h_100/rotate,90";
            GetObjectRequest request(BucketName, ObjectName);
            request.setProcess(Process);
            auto outcome = client.GetObject(request);
        
            /* Release network resources. */
            ShutdownSdk();
            return 0;
        }
        ```


## Implement permanent storage for a processed image {#section_rqc_yfd_lfb .section}

Use the following code to store a processed image in a bucket for permanent storage:

```
#include <alibabacloud/oss/OssClient.h>
#include <sstream>
using namespace AlibabaCloud::OSS;

int main(void)
{
     /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string SourceObjectName = "yourSourceObjectName";
    std::string TargetObjectName = "yourTargetObjectName";

     /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    /* Scale the image. */
    std::string Process = "image/resize,m_fixed,w_100,h_100";
    std::stringstream ss;
    ss  << Process 
    <<"|sys/saveas"
    << ",o_" << Base64EncodeUrlSafe(TargetObjectName)
    << ",b_" << Base64EncodeUrlSafe(BucketName);
    ProcessObjectRequest request(BucketName, SourceObjectName, ss.str());
    auto outcome = client.ProcessObject(request);
  
    /* Crop the image. */
    Process = "image/crop,w_100,h_100,x_100,y_100,r_1";
    ss.str("");
    ss  << Process 
    <<"|sys/saveas"
    << ",o_" << Base64EncodeUrlSafe(TargetObjectName)
    << ",b_" << Base64EncodeUrlSafe(BucketName);
    request = request(BucketName, SourceObjectName, ss.str());
    outcome = client.ProcessObject(request);
  
    /* Rotate the image. */
    Process = "image/rotate,90";
    ss.str("");
    ss  << Process 
    <<"|sys/saveas"
    << ",o_" << Base64EncodeUrlSafe(TargetObjectName)
    << ",b_" << Base64EncodeUrlSafe(BucketName);
    request = request(BucketName, SourceObjectName, ss.str());
    outcome = client.ProcessObject(request);
  
    /* Sharpen the image. */
    Process = "image/sharpen,100";
    ss.str("");
    ss  << Process 
    <<"|sys/saveas"
    << ",o_" << Base64EncodeUrlSafe(TargetObjectName)
    << ",b_" << Base64EncodeUrlSafe(BucketName);
    request = request(BucketName, SourceObjectName, ss.str());
    outcome = client.ProcessObject(request);
  
    /* Add the watermark. */
    Process = "image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ";
    ss.str("");
    ss  << Process 
    <<"|sys/saveas"
    << ",o_" << Base64EncodeUrlSafe(TargetObjectName)
    << ",b_" << Base64EncodeUrlSafe(BucketName);
    request = request(BucketName, SourceObjectName, ss.str());
    outcome = client.ProcessObject(request);
  
    /* Convert the image format. */
    Process = "image/format,png";
    ss.str("");
    ss  << Process 
    <<"|sys/saveas"
    << ",o_" << Base64EncodeUrlSafe(TargetObjectName)
    << ",b_" << Base64EncodeUrlSafe(BucketName);
    request = request(BucketName, SourceObjectName, ss.str());
    outcome = client.ProcessObject(request);

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

