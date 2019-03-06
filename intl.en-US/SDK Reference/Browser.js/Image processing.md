# Image processing {#concept_64054_zh .concept}

OSS Image Service \(IMG\) is an image processing service that features massive capacity, high security, low costs, and high reliability. By uploading and storing original images in OSS, you can process images anytime, anywhere, on any Internet device through a simple RESTful API.IMG offers image processing APIs.

For more information, see [Image processing](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#).

## Basic features of Image Service {#section_brj_5wg_kfb .section}

OSS IMG provides the following features:

-   [Obtain image information](../../../../../reseller.en-US/Data Processing/Image Processing/Obtain image information/Retrieve dominant image tones.md#)
-   [Image format conversion](../../../../../reseller.en-US/Data Processing/Image Processing/Convert formats/Format conversion.md#)
-   [Resize images](../../../../../reseller.en-US/Data Processing/Image Processing/Resize images.md#)
-   [Incircle](../../../../../reseller.en-US/Data Processing/Image Processing/Crop images/Incircle.md#)
-   [Adaptive orientation](../../../../../reseller.en-US/Data Processing/Image Processing/Rotate images/Adaptive orientation.md#)
-   [Brightness](../../../../../reseller.en-US/Data Processing/Image Processing/Apply effects/Brightness.md#)
-   [Add watermarks](../../../../../reseller.en-US/Data Processing/Image Processing/Add watermarks.md#): allows you to add images, texts, or image-text watermarks to another image.
-   [Customize image processing styles](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#)
-   Calling multiple image processing features through [Cascading Processing](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing access rules.md#)

## Usage of Image Service {#section_gc5_swg_kfb .section}

Image Service uses the standard HTTP GET request for access. All processing parameters are encoded in the QueyString in the URL.

If the ACL for an image is private read and write, only authorized users are allowed can access the image.

-   Anonymous access

    If the access permission of the image object is `public read`, as shown in the following table, you can access Image Service anonymously.

    |Bucket ACL|Object ACL|
    |:---------|:---------|
    |Public-read or Public-read-write|Default|
    |Any ACL|Public-read or Public-read-write|

    Access Image Service anonymously using the third-level domain in the following format:

    ```
    http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction>,<yourParamValue>
    ```

    |Parameter|Description|
    |:--------|:----------|
    |bucket|Name of the user’s bucket|
    |endpoint|The access domain name for the data center of the user’s bucket|
    |object|The image object uploaded by the user to the OSS|
    |image|The identifier reserved by Image Service|
    |action|Operations by the user on images, such as scaling, cropping, and rotating|
    |param|Parameters corresponding to the operations by the user on images|

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

        -   style: The identifier reserved by the system for the user’s custom style
        -   yourStyleName: specifies the name of a custom image style. It is the name specified in the rule that is created on the OSS console.
        For example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/oss-pic-style-w-100
        ```

    -   Cascade operations

        With the cascading processing, you can implement multiple operations on an image. The format is as follows:

        ```
        http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction1>,<yourParamValue1>/<yourAction2>,<yourParamValue2>/...
        ```

        For example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100/rotate,90
        ```

    -   Image Service also supports HTTPS access.

        For example,

        ```
        https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100
        ```

-   Authorized access

    For objects with private permissions, as shown in the following table, you must be authorized to access Image Service.

    |Bucket ACL|Object ACL|
    |:---------|----------|
    |private|default|
    |Any ACL|private|

    The Image Service URL code generated with signature is as follows:

    ```
    let OSS = require('ali-oss');
    let client = new OSS({
      accessKeyId: '<accessKeyId>',
      accessKeySecret: '<accessKeySecret>',
      bucket: '<bucketName>',
      endpoint: '<endpoint, 例如http://oss-cn-hangzhou.aliyuncs.com>'
    });
    // Expire after 10 minutes. The image processing style is: "image/resize,w_300".
    let signUrl = client.signatureUrl('example.jpg', {expires: 600, 'process' : 'image/resize,w_300'});
    console.log("signUrl="+signUrl);
    ```

    **Note:** 

    -   Authorized access supports `custom styles`, `HTTPS`, and `cascading processing`.
    -   Expiration time \(expires\) is measured in seconds.
-   SDK access

    You can directly use `SDK` to access and process image objects with any ACLs.

    **Note:** The image object processing using SDKs supports `custom styles`, `HTTPS`, and `cascading processing`.

    -   Basic operations

        Run the following code to perform basic operations on an image:

        ```language-JavaScript
        let OSS = require('ali-oss');
        
        let client = new OSS({
          accessKeyId: '<accessKeyId>',
          accessKeySecret: '<accessKeySecret>',
          bucket: '<bucketName>',
          endpoint: '<endpoint, 例如http://oss-cn-hangzhou.aliyuncs.com>'
        });
        
        // Scale the image.
        async function scale () {
          try {
            let result = await client.signatureUrl('example.jpg', {expires: 3600, process: 'image/resize,m_fixed,w_100,h_100'})
          } catch (e) {
            console.log(e);
          }
        }
        
        scale();
        
        // Crop the image.
        async function crop () {
          try {
            let result = await client.signatureUrl('example.jpg', {expires: 3600, process: 'image/crop,w_100,h_100,x_100,y_100,r_1'})
          } catch (e) {
            console.log(e);
          }
        }
        
        crop();
        
        // Rotate the image.
        async function rotate() {
          try {
            let result = await client.signatureUrl('example.jpg', {expires: 3600, process: 'image/rotate,90'})
          } catch (e) {
            console.log(e);
          }
        }
        
        rotate();
        
        // Sharpen the image.
        async function sharpen () {
          try {
            let result = await client.signatureUrl('example.jpg', {expires: 3600, process: 'image/sharpen,100'})
          } catch (e) {
            console.log(e);
          }
        }
        
        sharpen();
        
        // Add watermarks.
        async function watermark () {
          try {
            let result = await client.signatureUrl('example.jpg', {expires: 3600, process: 'image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ'})
          } catch (e) {
            console.log(e);
          }
        }
        
        watermark();
        
        // Convert the image format.
        async function format () {
          try {
            let result = await client.signatureUrl('example.jpg', {expires: 3600, process: 'image/format,png'})
          } catch (e) {
            console.log(e);
          }
        }
        
        format();
        
        // Obtain image information.
        async function info () {
          try {
            let result = await client.signatureUrl('example.jpg', {expires: 3600, process: 'image/info})
          } catch (e) {
            console.log(e)
          }
        }
        
        info();
        
        ```

    -   Customize image styles

        You can run the following code to customize an image style:

        ```language-JavaScript
        let OSS = require('ali-oss');
        
        let client = new OSS({
          accessKeyId: '<accessKeyId>',
          accessKeySecret: '<accessKeySecret>',
          bucket: '<bucketName>',
          endpoint: '<endpoint, 例如http://oss-cn-hangzhou.aliyuncs.com>'
        });
        
        // Custom styles
        async function style () {
          try {
            let result = await client.signatureUrl('example.jpg', {expires: 3600, process: 'style/example-sytle'});
          } catch (e) {
            console.log(e);
          }
        }
        
        style();
        
        ```

        **Note:** You must first log on to the OSS console to add the custom style 'example-style'.

    -   Cascade operations

        Run the following code to perform cascade operations on an image:

        ```language-JavaScript
        let OSS = require('ali-oss');
        
        let client = new OSS({
          accessKeyId: '<accessKeyId>',
          accessKeySecret: '<accessKeySecret>',
          bucket: '<bucketName>',
          endpoint: '<endpoint, for example, http://oss-cn-hangzhou.aliyuncs.com>'
        });
        
        // Cascading processing
        async function resize () {
          try {
            let result = await client.signatureUrl('example.jpg', {expires: 3600, process: 'image/resize,m_fixed,w_100,h_100/rotate,90'})
          } catch (e) {
            console.log(e);
          }
        }
        
        resize();
        
        ```


