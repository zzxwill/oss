# Image processing {#concept_agt_jgc_kfb .concept}

Image Processing \(IMG\) is a massive, secure, cost-effective and highly reliable image processing service. After source images are uploaded to OSS, you can process images on any Internet device at any anytime, from anywhere through simple RESTful APIs.

For more information about IMG, see [Image Processing](../../../../reseller.en-US/Image Processing Guide/Image processing.md#).

For the complete code of IMG, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/ImageSample.java).

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
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    // Customize the image style.
    String style = "image/resize,m_fixed,w_100,h_100/rotate,90";
    // Set the expiration time to 10 minutes.
    Date expiration = new Date(new Date().getTime() + 1000 * 60 * 10 );
    GeneratePresignedUrlRequest req = new GeneratePresignedUrlRequest(bucketName, objectName, HttpMethod.GET);
    req.setExpiration(expiration);
    req.setProcess(style);
    URL signedUrl = ossClient.generatePresignedUrl(req);
    System.out.println(signedUrl);
    // Close your OSSClient.
    ossClient.shutdown();sClient.shutdown();
    ```

-   Access with SDK

    You can use SDK to access and process an image object with any ACLs.

    SDK allows you to customize image styles, HTTPS access, and cascade operations.

    -   Basic operations

        Run the following code to perform basic operations on an image:

        ```
        // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        String objectName = "<yourObjectName>";
        // Create an OSSClient instance.
        OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
        // Scale the image.
        String style = "image/resize,m_fixed,w_100,h_100";
        GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
        request.setProcess(style);
        ossClient.getObject(request, new File("example-resize.jpg"));
        // Crop the image.
        style = "image/crop,w_100,h_100,x_100,y_100,r_1";
        request = new GetObjectRequest(bucketName, objectName);
        request.setProcess(style);
        ossClient.getObject(request, new File("example-crop.jpg"));
        // Rotate the image.
        style = "image/rotate,90";
        request = new GetObjectRequest(bucketName, objectName);
        request.setProcess(style);
        ossClient.getObject(request, new File("example-rotate.jpg"));
        // Sharpen the image.
        style = "image/sharpen,100"; 
        request = new GetObjectRequest(bucketName, objectName);
        request.setProcess(style);
        ossClient.getObject(request, new File("example-sharpen.jpg"));
        // Add watermarks.
        style = "image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ"; 
        request = new GetObjectRequest(bucketName, objectName);
        request.setProcess(style);
        ossClient.getObject(request, new File("example-watermark.jpg"));
        // Convert the image format.
        style = "image/format,png";
        request = new GetObjectRequest(bucketName, objectName);
        request.setProcess(style);
        ossClient.getObject(request, new File("example-format.png"));
        // Obtain image information.
        style = "image/info";
        request = new GetObjectRequest(bucketName, objectName);
        request.setProcess(style);
        ossClient.getObject(request, new File("example-info.txt"));
        // Close your OSSClient.
        ossClient.shutdown();
        ```

    -   Customize an image style

        Run the following code to customize an image style:

        ```
        // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        String objectName = "<yourObjectName>";
        
        // Create an OSSClient instance.
        OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
        
        // Customize the image style.
        String style = "style/<yourCustomStyleName>";
        GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
        request.setProcess(style);
        
        ossClient.getObject(request, new File("example-new.jpg"));
        
        // Close your OSSClient.
        ossClient.shutdown();
        ```

    -   Cascade operations

        Run the following code to perform cascade operations on an image:

        ```
        // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        String objectName = "<yourObjectName>";
        
        // Create an OSSClient instance.
        OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
        
        // Perform cascade operations.
        String style = "image/resize,m_fixed,w_100,h_100/rotate,90";
        GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
        request.setProcess(style);
        
        ossClient.getObject(request, new File("example-new.jpg"));
        
        // Disable the OSSClient.
        ossClient.shutdown();
        ```


## Image processing persistence {#section_rqc_yfd_lfb .section}

Run the following code for image processing persistence:

```
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance.
To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String sourceImage = "<yourSourceImageName>";

try {
	// Create an OSSClient instance.
	OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

	// Image processing persistence: Scale the image.
	StringBuilder sbStyle = new StringBuilder();
	Formatter styleFormatter = new Formatter(sbStyle);
	String styleType = "image/resize,m_fixed,w_100,h_100";
	String targetImage = "example-resize.png";
	styleFormatter.format("%s|sys/saveas,o_%s,b_%s", styleType,
			BinaryUtil.toBase64String(targetImage.getBytes()),
			BinaryUtil.toBase64String(bucketName.getBytes()));
	System.out.println(sbStyle.toString());
	ProcessObjectRequest request = new ProcessObjectRequest(bucketName, sourceImage, sbStyle.toString());
	GenericResult processResult = ossClient.processObject(request);
	String json = IOUtils.readStreamAsString(processResult.getResponse().getContent(), "UTF-8");
	processResult.getResponse().getContent().close();
	System.out.println(json);

	// Image processing persistence: Clip the image.
	sbStyle.delete(0, sbStyle.length());
	styleType = "image/crop,w_100,h_100,x_100,y_100,r_1";
	targetImage = "example-crop.png";
	styleFormatter.format("%s|sys/saveas,o_%s,b_%s", styleType,
			BinaryUtil.toBase64String(targetImage.getBytes()),
			BinaryUtil.toBase64String(bucketName.getBytes()));
	System.out.println(sbStyle.toString());
	request = new ProcessObjectRequest(bucketName, sourceImage, sbStyle.toString());
	processResult = ossClient.processObject(request);
	json = IOUtils.readStreamAsString(processResult.getResponse().getContent(), "UTF-8");
	processResult.getResponse().getContent().close();
	System.out.println(json);

	// Image processing persistence: Rotate the object.
	sbStyle.delete(0, sbStyle.length());
	styleType = "image/rotate,90";
	targetImage = "example-rotate.png";
	styleFormatter.format("%s|sys/saveas,o_%s,b_%s", styleType,
			BinaryUtil.toBase64String(targetImage.getBytes()),
			BinaryUtil.toBase64String(bucketName.getBytes()));
	request = new ProcessObjectRequest(bucketName, sourceImage, sbStyle.toString());
	processResult = ossClient.processObject(request);
	json = IOUtils.readStreamAsString(processResult.getResponse().getContent(), "UTF-8");
	processResult.getResponse().getContent().close();
	System.out.println(json);

	// Image processing persistence: Sharpen the image.
	sbStyle.delete(0, sbStyle.length());
	styleType = "image/sharpen,100";
	targetImage = "example-sharpen.png";
	styleFormatter.format("%s|sys/saveas,o_%s,b_%s", styleType,
			BinaryUtil.toBase64String(targetImage.getBytes()),
			BinaryUtil.toBase64String(bucketName.getBytes()));
	request = new ProcessObjectRequest(bucketName, sourceImage, sbStyle.toString());
	processResult = ossClient.processObject(request);
	json = IOUtils.readStreamAsString(processResult.getResponse().getContent(), "UTF-8");
	processResult.getResponse().getContent().close();
	System.out.println(json);

	// Image processing persistence: Add watermarks.
	sbStyle.delete(0, sbStyle.length());
	styleType = "image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ";
	targetImage = "example-watermark.png";
	styleFormatter.format("%s|sys/saveas,o_%s,b_%s", styleType,
			BinaryUtil.toBase64String(targetImage.getBytes()),
			BinaryUtil.toBase64String(bucketName.getBytes()));
	request = new ProcessObjectRequest(bucketName, sourceImage, sbStyle.toString());
	processResult = ossClient.processObject(request);
	json = IOUtils.readStreamAsString(processResult.getResponse().getContent(), "UTF-8");
	processResult.getResponse().getContent().close();
	System.out.println(json);

	// Image processing persistence: Convert the image format.
	sbStyle.delete(0, sbStyle.length());
	styleType = "image/format,jpg";
	targetImage = "example-formatconvert.jpg";
	styleFormatter.format("%s|sys/saveas,o_%s,b_%s", styleType,
			BinaryUtil.toBase64String(targetImage.getBytes()),
			BinaryUtil.toBase64String(bucketName.getBytes()));
	request = new ProcessObjectRequest(bucketName, sourceImage, sbStyle.toString());
	processResult = ossClient.processObject(request);
	json = IOUtils.readStreamAsString(processResult.getResponse().getContent(), "UTF-8");
	processResult.getResponse().getContent().close();
	System.out.println(json);

} catch (Exception e) {
	e.printStackTrace();
}
```

## IMG tools {#section_h3w_5mh_kfb .section}

You can use the IMG viewer [ImageStyleViever](https://gosspublic.alicdn.com/image/index.html) to directly view the IMG result.

