# Image processing {#concept_47660_zh .concept}

Image Processing \(IMG\) is a massive, secure, cost-effective, and highly reliable image processing service. After source images are uploaded to OSS, you can process images on any Internet device at any anytime and from anywhere through simple RESTful APIs.

For more information about IMG, see [Image Processing](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#).

For the complete code of IMG, see [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/image.py).

## Basic features {#section_rqw_skn_kfb .section}

OSS provides the following IMG features:

-   [Retrieve dominant image tones](../../../../../reseller.en-US/Data Processing/Image Processing/Obtain image information/Retrieve dominant image tones.md#)
-   [Format conversion](../../../../../reseller.en-US/Data Processing/Image Processing/Convert formats/Format conversion.md#)
-   [Resize images](../../../../../reseller.en-US/Data Processing/Image Processing/Resize images.md#)
-   [Incircle](../../../../../reseller.en-US/Data Processing/Image Processing/Crop images/Incircle.md#)
-   [Adaptive orientation](../../../../../reseller.en-US/Data Processing/Image Processing/Rotate images/Adaptive orientation.md#)
-   [Brightness](../../../../../reseller.en-US/Data Processing/Image Processing/Apply effects/Brightness.md#)
-   [Add watermarks](../../../../../reseller.en-US/Data Processing/Image Processing/Add watermarks.md#): allows you to add images, texts, or image-text watermarks to another image.
-   [Image processing](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#)
-   [Image processing access rules](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing access rules.md#): calls multiple IMG functions.

## Usage { .section}

IMG uses standard HTTP GET requests. You can configure IMG parameters in the QueryString included in the URL.of an image.

If the ACL for an image is private read and write, only authorized users are allowed can access the image.

-   Anonymous access

    You can use a level-3 domain name in the following format to access a processed image:

    ```
    http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction>,<yourParamValue>
    ```

    |Parameter|Description|
    |:--------|:----------|
    |bucket|Specifies the name of a bucket.|
    |endpoint|Specifies the endpoint used to access the region that the bucket is created.|
    |object|Specifies the name of an image object.|
    |image|Specifies the reserved identifier for IMG.|
    |action|Specifies the operation performed on an image, such as scaling, cropping, and rotating.|
    |param|Specifies the parameter that indicates the operation performed on an image.|

    -   Basic operations

        For example, scale an image to a width of 100 px and adjust the height based on the ratio.

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100
        ```

    -   Customize image styles

        You can use a level-3 domain name in the following format to access a processed image:

        ```
        http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=style/<yourStyleName>
        ```

        -   style: specifies the reserved identifier of a customized image style.
        -   yourStyleName: specifies the name of a custom image style. It is the name specified in the rule that is created on the OSS console.
        For example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/oss-pic-style-w-100
        ```

    -   Cascade operations

        Cascade operations allow you to perform multiple operations on an image in sequence.

        ```
        http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction1>,<yourParamValue1>/<yourAction2>,<yourParamValue2>/...
        ```

        For example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100/rotate,90
        ```

    -   Access through HTTPS

        IMG allows you to access images through the HTTPS protocol. For example:

        ```
        https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100
        ```

-   Authorized access

    Authorized access allows you to customize an image style, access images through the HTTPS protocol, and perform cascade operations.

    Run the following code to generate a signed URL for IMG:

    ```language-python
    # -*- coding: utf-8 -*-
    import oss2
    
    # This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    endpoint = 'http://oss-cn-hangzhou.aliyuncs.com'
    # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We strongly recommend that you create a RAM user and use it for API access and daily operations and maintenance. Log on to https://ram.console.aliyun.com to create a RAM user.
    access_key_id = '<yourAccessKeyId>'
    access_key_secret = '<yourAccessKeySecret>'
    bucket_name = '<yourBucketName>'
    key = 'example.jpg'
    
    # Creates a bucket instance. All object-related methods must be called by the bucket instance.
    bucket = oss2. Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
    
    # Uploads a sample image.
    bucket.put_object_from_file(key, 'example.jpg')
    
    # Generates a signed URL and sets the expiration time to 10 minutes. The unit of expiration time is second.
    style = 'image/resize,m_fixed,w_100,h_100/rotate,90'
    url = bucket.sign_url('GET', key, 10 * 60, params={'x-oss-process': style})
    print(url)
    
    ```

-   Access image objects with SDK

    You can use OSS SDK to access and process an image object.

    SDK allows you to customize image styles, access images through the HTTPS protocol, and perform cascade operations.

    -   Basic operations

        The get\_object and get\_object\_to\_file methods support image processing.

        Use the following code to perform basic operations on an image:

        ```language-python
        # -*- coding: utf-8 -*-
        import os
        import oss2
        
        # This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
        endpoint = 'http://oss-cn-hangzhou.aliyuncs.com'
        # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We strongly recommend that you create a RAM user and use it for API access and daily operations and maintenance. Log on to https://ram.console.aliyun.com to create a RAM user.
        access_key_id = '<yourAccessKeyId>'
        access_key_secret = '<yourAccessKeySecret>'
        bucket_name = '<yourBucketName>'
        key = 'example.jpg'
        new_pic = 'example-new.jpg'
        
        # Creates a bucket instance. All object-related methods must be called by the bucket instance.
        bucket = oss2. Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
        
        # Uploads a sample image.
        bucket.put_object_from_file(key, 'example.jpg')
        
        # Scales the image.
        style = 'image/resize,m_fixed,w_100,h_100'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Crops the image.
        style = 'image/crop,w_100,h_100,x_100,y_100,r_1'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Rotates the image.
        style = 'image/rotate,90'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Sharpens the image.
        style = 'image/sharpen,100'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Adds watermarks.
        style = 'image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Converts the image format.
        style = 'image/format,png'
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Deletes the sample image.
        bucket.delete_object(key)
        # Clears local files.
        os.remove(new_pic)
        
        ```

    -   Customize an image style

        You can run the following code to customize an image style:

        ```language-python
        # -*- coding: utf-8 -*-
        import os
        import oss2
        
        # This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
        endpoint = 'http://oss-cn-hangzhou.aliyuncs.com'
        # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We strongly recommend that you create a RAM user and use it for API access and daily operations and maintenance. Log on to https://ram.console.aliyun.com to create a RAM account.
        access_key_id = '<yourAccessKeyId>'
        access_key_secret = '<yourAccessKeySecret>'
        bucket_name = '<yourBucketName>'
        key = 'example.jpg'
        new_pic = 'example-new.jpg'
        
        # Creates a bucket instance. All object-related methods must be called by the bucket instance.
        bucket = oss2. Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
        
        # Uploads a sample image.
        bucket.put_object_from_file(key, 'example.jpg')
        
        # Customizes the image style.
        style = 'style/oss-pic-style-w-100'
        
        # Processes the image.
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Deletes the sample image.
        bucket.delete_object(key)
        # Clears local files.
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
        # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We strongly recommend that you create a RAM user and use it for API access and daily operations and maintenance. Log on to https://ram.console.aliyun.com to create a RAM user.
        access_key_id = '<yourAccessKeyId>'
        access_key_secret = '<yourAccessKeySecret>'
        bucket_name = '<yourBucketName>'
        key = 'example.jpg'
        new_pic = 'example-new.jpg'
        
        # Creates a bucket instance. All object-related methods must be called by the bucket instance.
        bucket = oss2. Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
        
        # Uploads a sample image.
        bucket.put_object_from_file(key, 'example.jpg')
        
        # Performs cascade operations.
        style = 'image/resize,m_fixed,w_100,h_100/rotate,90'
        
        # Processes the image.
        bucket.get_object_to_file(key, new_pic, process=style)
        
        # Deletes the sample image.
        bucket.delete_object(key)
        # Clears local files.
        os.remove(new_pic)
        
        ```


## Image processing persistence {#section_pdl_dpw_mfb .section}

OSS provides the “saveas” operation for data processing. With this feature, you can save the processing result to a specified bucket as resources and assign the result with a specified key. After the resources are saved, you can access the resources directly by specifying the bucket to speed up resource download. This feature applies to ultra-large image cropping or other high-latency operations.

Run the following code for image processing persistence:

```
# -*- coding: utf-8 -*-
import os
import base64
import oss2

# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
endpoint = 'http://oss-cn-hangzhou.aliyuncs.com'
# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. Instead, we strongly recommend that you create a RAM user and use it for API access and daily operations and maintenance. Log on to https://ram.console.aliyun.com to create a RAM account.
# source_bucket_name and taget_bucket_name are the same bucket. 
access_key_id = '<yourAccessKeyId>'
access_key_secret = '<yourAccessKeySecret>'
source_bucket_name = '<yourBucketName>'
taget_bucket_name = '<yourTargetBucketName>'
source_image_name = 'example.jpg'

# Creates a bucket instance. All object-related methods must be called by the bucket instance.
bucket = oss2. Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, source_bucket_name)

# Scales the image.
style = 'image/resize,m_fixed,w_100,h_100'
Target_image_name = 'example-resize.jpg'
process = "{0}|sys/saveas,o_{1},b_{2}".format(style, 
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(target_image_name))),
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(taget_bucket_name))))
result = bucket.process_object(source_image_name, process)
print(result)

# Crops the image.
style = 'image/crop,w_100,h_100,x_100,y_100,r_1'
target_image_name = 'example-crop.jpg'
process = "{0}|sys/saveas,o_{1},b_{2}".format(style, 
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(target_image_name))),
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(taget_bucket_name))))
result = bucket.process_object(source_image_name, process)
print(result)

# Rotates the image.
style = 'image/rotate,90'
target_image_name = 'example-rotate.jpg'
process = "{0}|sys/saveas,o_{1},b_{2}".format(style, 
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(target_image_name))),
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(taget_bucket_name))))
result = bucket.process_object(source_image_name, process)
print(result)

# Sharpens the image.
style = 'image/sharpen,100'
target_image_name = 'example-sharpen.jpg'
process = "{0}|sys/saveas,o_{1},b_{2}".format(style, 
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(target_image_name))),
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(taget_bucket_name))))
result = bucket.process_object(source_image_name, process)
print(result)

# Adds watermarks.
style = 'image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ'
target_image_name = 'example-watermark.jpg'
process = "{0}|sys/saveas,o_{1},b_{2}".format(style, 
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(target_image_name))),
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(taget_bucket_name))))
result = bucket.process_object(source_image_name, process)
print(result)

# Converts the image format.
style = 'image/format,png'
target_image_name = 'example-formatconvert.png'
process = "{0}|sys/saveas,o_{1},b_{2}".format(style, 
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(target_image_name))),
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(taget_bucket_name))))
result = bucket.process_object(source_image_name, process)
print(result)
```

