# Image processing {#concept_50039_zh .concept}

Alibaba Cloud OSS provides image processing service featuring massive processing power, security, low costs, and high reliability. By uploading and storing source images in OSS, you can process images anytime, anywhere, and on any Internet device through a simple RESTful interface.

## Image processing basic features {#section_brj_5wg_kfb .section}

Image processing provides the following features:

-   [Retrieve dominant image tones](../../../../../reseller.en-US/Data Processing/Image Processing/Obtain image information/Retrieve dominant image tones.md#)
-   [Format conversion](../../../../../reseller.en-US/Data Processing/Image Processing/Convert formats/Format conversion.md#)
-   [Resize images](../../../../../reseller.en-US/Data Processing/Image Processing/Resize images.md#)
-   [Incircle](../../../../../reseller.en-US/Data Processing/Image Processing/Crop images/Incircle.md#)
-   [Adaptive orientation](../../../../../reseller.en-US/Data Processing/Image Processing/Rotate images/Adaptive orientation.md#)
-   [Brightness](../../../../../reseller.en-US/Data Processing/Image Processing/Apply effects/Brightness.md#)
-   [Add watermarks](../../../../../reseller.en-US/Data Processing/Image Processing/Add watermarks.md#): allows you to add images, texts, or image-text watermarks to another image.
-   [Customize image processing styles](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#)
-   Calling multiple image processing features through [Image service access rules](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing access rules.md#).

## Usage of image processing {#section_gc5_swg_kfb .section}

Image processing uses the standard HTTP GET request for access. All processing parameters are encoded in the QueyString in the URL.

If the ACL for an image is private read and write, only authorized users are allowed can access the image.

-   Anonymous access

    If the access permission of the image object is `public read`, as shown in the following table, you can access image processing anonymously.

    |Bucket ACL|Object ACL|
    |:---------|:---------|
    |Public-read or Public-read-write|Default|
    |Any ACL|Public-read or Public-read-write|

    Access image processing anonymously using the third-level domain in the following format:

    ```
    http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction>,<yourParamValue>
    ```

    |Parameter|Description|
    |:--------|:----------|
    |bucket|Name of the user’s bucket|
    |endpoint|The access domain name for the data center of the user’s bucket|
    |object|The image object uploaded to the OSS|
    |image|The identifier reserved by the image processing|
    |action|Operations on images, such as scaling, cropping, and rotating|
    |param|Corresponding to the operations on the image|

    -   Basic operations

        For example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100
        ```

    -   Custom style.

        Access image processing anonymously using the third-level domain in the following format:

        ```
        http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=style/<yourStyleName>
        ```

        -   Style: The identifier reserved by the stem for the user’s custom style
        -   yourStyleName: Name of the custom style, that is, the rule name of the console-defined style
        For example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/oss-pic-style-w-100
        ```

    -   Through Image Service access rules,

        you can implement multiple operations on an image. The format is as follows:

        ```
        http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction1>,<yourParamValue1>/<yourAction2>,<yourParamValue2>/...
        ```

        For example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100/rotate,90
        ```

    -   Image processing also supports HTTPS access.

        For example:

        ```
        https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100
        ```

-   Authorized access

    For objects with private permissions, as shown in the following table, you must be authorized to access image processing:

    |Bucket ACL|Object ACL|
    |:---------|:---------|
    |private|default|
    |Any ACL|private|

    The image processing URL code generated with signature is as follows:

    ```
    let OSS = require('ali-oss');
    let client = new OSS({
      accessKeyId: '<accessKeyId>',
      accessKeySecret: '<accessKeySecret>',
      bucket: '<bucketName>',
      endpoint: '<endpoint, 例如http://oss-cn-hangzhou.aliyuncs.com>'
    });
    // Expired for 10 minutes. The image processing style is: "image/resize,w_300".
    let signUrl = client.signatureUrl('example.jpg', {expires: 600, 'process' : 'image/resize,w_300'});
    console.log("signUrl="+signUrl);
    ```

    **Note:** 

    -   Authorized access supports `custom styles`, `HTTPS`, and `cascading processing`.
    -   Expiration time \(expires\) is measured in seconds.
-   SDK access

    You can directly use SDK to access and process image objects with any permissions.

    Image object processing through SDK supports custom styles, HTTPS, and Image Service access rules.

    -   Basic operations

        Run the following code to perform basic operations on an image:

        ```
        let OSS = require('ali-oss');
        let client = new OSS({
          accessKeyId: '<accessKeyId>',
          accessKeySecret: '<accessKeySecret>',
          bucket: '<bucketName>',
          endpoint: '<endpoint, for example, http://oss-cn-hangzhou.aliyuncs.com>'
        });
        // Scale the image.
        async function scale () {
          try {
            let result = await client.get('example.jpg', './example-resize.jpg', { process: 'image/resize,m_fixed,w_100,h_100'});
          } catch (e) {
            console.log(e);
          }
        }
        // Crop the image.
        async function cut () {
          try {
             let result = await client.get('example.jpg', './example-crop.jpg', { process: 'image/crop,w_100,h_100,x_100,y_100,r_1'});
          } catch (e) {
            console.log(e)
          }
        }
        // Rotate the image.
        async function rotate () {
          try {
            let result = await client.get('example.jpg', './example-rotate.jpg', { process: 'image/rotate,90'});
          } catch (e) {
            console.log(e);
          }
        }
        // Sharpen the image.
        async function sharpen () {
          try {
            let result = yield client.get('example.jpg', './example-sharpen.jpg', { process: 'image/sharpen,100'});
          } catch (e) {
            console.log(e);
          }
        }
        // Add watermarks.
        async function watermark () {
          try {
            let result = yield client.get('example.jpg', './example-watermark.jpg', { process: 'image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ'});
          } catch (e) {
            console.log(e);
          }
        }
        // Convert the image format.
        async function format () {
          try {
            let result = await client.get('example.jpg', './example-format.jpg', { process: 'image/format,png'});
          } catch (e) {
            console.log(e);
          }
        }
        // Obtain image information.
        async function info () {
          try {
            let result = await client.get('example.jpg', './example-info.txt', {process: 'image/info'});
          } catch (e) {
            console.log(e);
          }
        }
        ```

    -   Custom styles

        **Note:** You must first log on to the OSS console to add the custom style 'example-style'.

        You can run the following code to customize an image style:

        ```
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
            let result = await client.get('example.jpg', './example-custom-style.jpg', {process: 'style/example-sytle"'});
          } catch (e) {
            console.log(e);
          }
        }
        style();
        ```

    -   Cascading processing

        Run the following code to perform cascade operations on an image:

        ```
        let OSS = require('ali-oss');
        let client = new OSS({
          accessKeyId: '<accessKeyId>',
          accessKeySecret: '<accessKeySecret>',
          bucket: '<bucketName>',
          endpoint: '<endpoint, for example, http://oss-cn-hangzhou.aliyuncs.com>'
        });
        // Cascading processing
        async function cascade () {
          try {
            let result = await client.get('example.jpg', './example-cascade.jpg', {process: 'image/resize,m_fixed,w_100,h_100/rotate,90'});
          } catch (e) {
            console.log(e);
          }
        }
        cascade();
        ```


