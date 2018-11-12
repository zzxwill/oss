# Image processing {#concept_48309_zh .concept}

Image Processing \(IMG\) is a massive, secure, cost-effective and highly reliable image processing service. After source images are uploaded to OSS, you can process images on any Internet device at any anytime, from anywhere through simple RESTful APIs.

For more information about IMG, see [Image Processing](../../../../reseller.en-US/Image Processing Guide/Image processing.md#).

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

## Usage {#section_gc5_swg_kfb .section}

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

        For example, scale an image to a width of 100 px. Adjust the height based on the ratio.

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

    ```
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
    var endpoint = "<yourEndpoint>";
    var accessKeyId = "<yourAccessKeyId>";
    var accessKeySecret = "<yourAccessKeySecret>";
    var bucketName = "<yourBucketName>";
    var objectName = "<yourObjectName>";
    // Create an OSSClient instance.
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
    try
    {
        var process = "image/resize,m_fixed,w_100,h_100";
        var req = new GeneratePresignedUriRequest(bucketName, objectName, SignHttpMethod.Get)
        {
            Expiration = DateTime.Now.AddHours(1),
            Process = process
        };
        // Generat a signed URI.
        var uri = client.GeneratePresignedUri(req);
        Console.WriteLine("Generate Presigned Uri:{0} with process:{1} succeeded ", uri, process);
    }
    catch (OssException ex)
    {
        Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
            ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Failed with error info: {0}", ex.Message);
    }
    ```

-   Access with SDK

    You can use SDK to access and process an image object.

    SDK allows you to customize image styles, HTTPS access, and cascade operations.

    -   Basic operations

        Run the following code to perform basic operations on an image:

        ```
        using System;
        using System.IO;
        using Aliyun.OSS;
        using Aliyun.OSS.Common;
        using Aliyun.OSS.Util;
        namespace ImageProcess
        {
            class Program
            {
                static void Main(string[] args)
                {
                    Program.ImageProcess();
                    Console.ReadKey();
                }
                public static void ImageProcess()
                {
                    var endpoint = "<yourEndpoint>";
                    var accessKeyId = "<yourAccessKeyId>";
                    var accessKeySecret = "<yourAccessKeySecret>";
                    var bucketName = "<yourBucketName>";
                    var objectName = "<yourObjectName>";
                    var localImageFilename = "<yourLocalImageFilename>";
                    var downloadDir = "<yourDownloadDir>";
                    // Create an OSSClient instance.
                    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
                    try
                    {
                        client.PutObject(bucketName, objectName, localImageFilename);
                        // Scale the image.
                        var process = "image/resize,m_fixed,w_100,h_100";
                        var ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                        WriteToFile(downloadDir + "/sample-resize.jpg", ossObject.Content);
                        // Crop the image.
                        process = "image/crop,w_100,h_100,x_100,y_100,r_1";
                        ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                        WriteToFile(downloadDir + "/sample-crop.jpg", ossObject.Content);
                        // Rotate the image.
                        process = "image/rotate,90";
                        ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                        WriteToFile(downloadDir + "/sample-rotate.jpg", ossObject.Content);
                        // Sharpen the image.
                        process = "image/sharpen,100";
                        ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                        WriteToFile(downloadDir + "/sample-sharpen.jpg", ossObject.Content);
                        // Add watermarks.
                        process = "image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ";
                        ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                        WriteToFile(downloadDir + "/sample-watermark.jpg", ossObject.Content);
                        // Convert the image format.
                        process = "image/format,png";
                        ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                        WriteToFile(downloadDir + "/sample-format.png", ossObject.Content);
                        // Obtain image information.
                        process = "image/info";
                        ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                        WriteToFile(downloadDir + "/sample-info.txt", ossObject.Content);
                        Console.WriteLine("Get Object:{0} with process:{1} succeeded ", objectName, process);
                    }
                    catch (OssException ex)
                    {
                        Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
                            ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
                    }
                    catch (Exception ex)
                    {
                        Console.WriteLine("Failed with error info: {0}", ex.Message);
                    }
                }
                private static void WriteToFile(string filePath, Stream stream)
                {
                    using (var requestStream = stream)
                    {
                        using (var fs = File.Open(filePath, FileMode.OpenOrCreate))
                        {
                            IoUtils.WriteTo(stream, fs);
                        }
                    }
                }
            }
        }
        ```

    -   Customize an image style

        Use the following code to customize an image style:

        ```
        using System;
        using System.IO;
        using Aliyun.OSS;
        using Aliyun.OSS.Common;
        using Aliyun.OSS.Util;
        namespace ImageProcessCustom
        {
            class Program
            {
                static void Main(string[] args)
                {
                    Program.ImageProcessCustomStyle();
                    Console.ReadKey();
                }
                public static void ImageProcessCustomStyle()
                {
                    var endpoint = "<yourEndpoint>";
                    var accessKeyId = "<yourAccessKeyId>";
                    var accessKeySecret = "<yourAccessKeySecret>";
                    var bucketName = "<yourBucketName>";
                    var objectName = "<yourObjectName>";
                    var localImageFilename = "<yourLocalImageFilename>";
                    var downloadDir = "<yourDownloadDir>";
                    // Create an OSSClient instance.
                    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
                    try
                    {
                        client.PutObject(bucketName, objectName, localImageFilename);
                        // Customize the image style: "style/<yourCustomStyleName>".
                        var process = "style/oss-pic-style-w-100";
                        var ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                        WriteToFile(downloadDir + "/sample-style.jpg", ossObject.Content);
                        Console.WriteLine("Get Object:{0} with process:{1} succeeded ", objectName, process);
                    }
                    catch (OssException ex)
                    {
                        Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
                            ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
                    }
                    catch (Exception ex)
                    {
                        Console.WriteLine("Failed with error info: {0}", ex.Message);
                    }
                }
                private static void WriteToFile(string filePath, Stream stream)
                {
                    using (var requestStream = stream)
                    {
                        using (var fs = File.Open(filePath, FileMode.OpenOrCreate))
                        {
                            IoUtils.WriteTo(stream, fs);
                        }
                    }
                }
            }
        }
        ```

    -   Cascade operations

        Use the following code to perform cascade operations on an image:

        ```
        using System;
        using System.IO;
        using Aliyun.OSS;
        using Aliyun.OSS.Common;
        using Aliyun.OSS.Util;
        namespace ImageProcessCascade
        {
            class Program
            {
                static void Main(string[] args)
                {
                    Program.ImageProcessCascade();
                    Console.ReadKey();
                }
                public static void ImageProcessCascade()
                {
                    var endpoint = "<yourEndpoint>";
                    var accessKeyId = "<yourAccessKeyId>";
                    var accessKeySecret = "<yourAccessKeySecret>";
                    var bucketName = "<yourBucketName>";
                    var objectName = "<yourObjectName>";
                    var localImageFilename = "<yourLocalImageFilename>";
                    var downloadDir = "<yourDownloadDir>";
                    // Create an OSSClient instance.
                    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
                    try
                    {
                        client.PutObject(bucketName, objectName, localImageFilename);
                        // Perform cascade operations.
                        var process = "image/resize,m_fixed,w_100,h_100/rotate,90";
                        var ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                        WriteToFile(downloadDir + "/sample-style.jpg", ossObject.Content);
                        Console.WriteLine("Get Object:{0} with process:{1} succeeded ", objectName, process);
                    }
                    catch (OssException ex)
                    {
                        Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
                            ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
                    }
                    catch (Exception ex)
                    {
                        Console.WriteLine("Failed with error info: {0}", ex.Message);
                    }
                }
                private static void WriteToFile(string filePath, Stream stream)
                {
                    using (var requestStream = stream)
                    {
                        using (var fs = File.Open(filePath, FileMode.OpenOrCreate))
                        {
                            IoUtils.WriteTo(stream, fs);
                        }
                    }
                }
            }
        }
        ```


## IMG tools {#section_h3w_5mh_kfb .section}

You can use the IMG viewer [ImageStyleViever](https://gosspublic.alicdn.com/image/index.html) to directly view the IMG result.

