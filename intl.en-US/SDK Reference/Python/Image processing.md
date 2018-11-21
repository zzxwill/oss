# Image processing {#concept_47660_zh .concept}

Image Processing \(IMG\) is a massive, secure, cost-effective and highly reliable image processing service. After source images are uploaded to OSS, you can process images on any Internet device at any anytime, from anywhere through simple RESTful APIs.

For more information about IMG, see [Image Processing](../../../../reseller.en-US/Image Processing Guide/Image processing.md#).

For the complete code of IMG, see [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/image.py).

## Basic features {#section_rqw_skn_kfb .section}

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

    ```language-python
    # -*- coding: utf-8 -*-
    import oss2
    
    # This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    endpoint = 'http://oss-cn-hangzhou.aliyuncs.com'
    # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    access_key_id = '<yourAccessKeyId>'
    access_key_secret = '<yourAccessKeySecret>'
    bucket_name = '<yourBucketName>'
    key = 'example.jpg'
    
    # Creates a bucket instance. All object-related methods must be called by the bucket instance.
    bucket = oss2.Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
    
    # Upload a sample image.
    bucket.put_object_from_file(key, 'example.jpg')
    
    # Generate a signed URL. Set the expiration time to 10 minutes. The unit of expiration time is second.
    style = 'image/resize,m_fixed,w_100,h_100/rotate,90'
    url = bucket.sign_url('GET', key, 10 * 60, params={'x-oss-process': style})
    print(url)
    
    ```

-   Access with SDK

    You can use SDK to access and process an image object.

    SDK allows you to customize image styles, HTTPS access, and cascade operations.

    -   Basic operations

        get\_object and get\_object\_to\_file support image processing.

        Run the following code to perform basic operations on an image:

        ```language-python
        # -*- coding: utf-8 -*-
        import os
        import oss2
        
        # This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
        endpoint = 'http://oss-cn-hangzhou.aliyuncs.com'
        # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
        access_key_id = '<yourAccessKeyId>'
        access_key_secret = '<yourAccessKeySecret>'
        bucket_name = '<yourBucketName>'
        key = 'example.jpg'
        new_pic = 'example-new.jpg'
        
        # Creates a bucket instance. All object-related methods must be called by the bucket instance.
        bucket = oss2.Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
        
        # Upload a sample image.
        bucket.put_object_from_file(key, 'example.jpg')
        
        # Scale the image.
        style = 'image/resize,m_fixed,w_100,h_100'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Crop the image.
        style = 'image/crop,w_100,h_100,x_100,y_100,r_1'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Rotate the image.
        style = 'image/rotate,90'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Sharpen the image.
        style = 'image/sharpen,100'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Add watermarks.
        style = 'image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Convert the image format.
        style = 'image/format,png'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Delete the sample image.
        bucket.delete_object(key)
        # Clear the local file.
        os.remove(new_pic)
        
        ```

    -   Customize an image style

        The following code is used to customize the image style:

        ```language-python
        # -*- coding: utf-8 -*-
        import os
        import oss2
        
        # This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
        endpoint = 'http://oss-cn-hangzhou.aliyuncs.com'
        # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
        access_key_id = '<yourAccessKeyId>'
        access_key_secret = '<yourAccessKeySecret>'
        bucket_name = '<yourBucketName>'
        key = 'example.jpg'
        new_pic = 'example-new.jpg'
        
        # Creates a bucket instance. All object-related methods must be called by the bucket instance.
        bucket = oss2.Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
        
        # Upload a sample image.
        bucket.put_object_from_file(key, 'example.jpg')
        
        # Customize the image style.
        style = 'style/oss-pic-style-w-100'
        
        # Process the image.
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Delete the sample image.
        bucket.delete_object(key)
        # Clear the local file.
        os.remove(new_pic)
        
        ```

    -   Cascade operations

        Run the following code to perform cascade operations on an image:

        ```language-python
        # -*- coding: utf-8 -*-
        import os
        import oss2
        
        # This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
        endpoint = 'http://oss-cn-hangzhou.aliyuncs.com'
        # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
        access_key_id = '<yourAccessKeyId>'
        access_key_secret = '<yourAccessKeySecret>'
        bucket_name = '<yourBucketName>'
        key = 'example.jpg'
        new_pic = 'example-new.jpg'
        
        # Creates a bucket instance. All object-related methods must be called by the bucket instance.
        bucket = oss2.Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
        
        # Upload a sample image.
        bucket.put_object_from_file(key, 'example.jpg')
        
        // Perform cascade operations.
        style = 'image/resize,m_fixed,w_100,h_100/rotate,90'
        
        # Process the image.
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Delete the sample image.
        bucket.delete_object(key)
        # Clear the local file.
        os.remove(new_pic)
        
        ```


## IMG tools { .section}

You can use the IMG viewer [ImageStyleViever](https://gosspublic.alicdn.com/image/index.html) to directly view the IMG result.

