# Image processing access rules {#concept_d4l_5fn_vdb .concept}

In Image Service, URLs are accessed with standard HTTP GET requests, and all processing parameters are in the QueyString of the URL.

## Request for thumbnails through processing parameters {#section_xzt_vfn_vdb .section}

If you want to have a source image processed and then returned, the following two formats are available:

-   URL

    Access through a third-level domain name: http://bucket.<endpoint\>/object?x-oss-process=image/action,parame\_value

    -   Bucket: your Image Service channel.
    -   endpoint: the access domain name for a Bucket’s data center.
    -   Object: In Image Service, an Object is the basic data unit for operating images. It is the same as the Object specified for the OSS instance. The maximum size of a single Object \(that is, each image\) is 20 MB.
    -   action: the operation to be performed on the image.
    -   parame: the parameter which indicates the operation to be performed on the image.
-   Combination of multiple actions

    Multiple actions are executed in sequence. For example, `image/resize,w_200/rotate,90` has the effect of scaling down an image to 200 in width and then rotating the image 90 degrees.


**Example**

Assume that the requested Bucket is image-demo and located in China East 1 \(Hangzhou\), with the domain name oss-cn-hangzhou.aliyuncs.com, and the requested image is example.jpg. The URL format for scaling down the image to 200 in width is:

```
http://**image-demo**.oss-cn-hangzhou.aliyuncs.com/**example.jpg**?x-oss-process=**image**/resize,w_200
```

The URL format for HTTPS access is:

```
https://**image-demo**.oss-cn-hangzhou.aliyuncs.com/**example.jpg**?x-oss-process=**image**/resize,w_200
```

The URL format for access through a custom domain name is:

```
http://userdomain/object?x-oss-process=image/action,parame_value
```

## Request for thumbnails through styles {#section_cr1_l3n_vdb .section}

**Style**

Image Service allows you to save image processing operations and parameters as an alias, that is, a style. With styles, a series of operations can be achieved through a very short URL.

-   A Channel can have multiple styles. Currently, a Channel is allowed to have up to 50 styles.
-   A style can be applied to change all Objects in a Channel. For example, if style abc is in Channel A and the style content is 100w.jpg \(scaled to 100 in width and saved as a .jpg file\), style abc can be applied to all the Objects in Channel A to scale them to 100 in width and saved them as .jpg files.
-   A style is only effective within a Channel, that is, the Objects in Channel A cannot use any style in Channel B.

Style naming conventions:

-   A name can be 1 to 63 characters in length.
-   Only numbers, upper-case or lower-case letters, underscores \( \_ \), hyphens \(-\), and periods \(.\) are permitted.

**Channel**

A channel is a namespace of image processing, and the management entity for billing, permission control, logging, and other advanced functions. An image name is globally unique in Image Service and cannot be modified. You can create up to 10 Channels, but the number of Objects in each Channel is not limited.

Image processing data centers correspond to the OSS data centers. If you create a Bucket in an OSS data center and then activate Image Service, the corresponding Channel belongs to this data center. **Currently, a Channel corresponds to a Bucket in the OSS instance, that is, you can only create a Channel of the same name as a Bucket that you have created on the OSS instance.**

Channel naming conventions:

-   Only lower-case letters, numbers, and hyphens \(-\) are permitted.
-   It must start and end with a lower-case letter or number.
-   The length must be 3–63 bytes.

To simplify the process, you can save a specific processing method as a style. Later, you just need to specify a style to call the same processing method. The URL format for image processing by style is as follows:

http://userdomain/object?x-oss-process=style/name

**Example**

The preceding processing parameters can be saved as the style style-example. Assume that the requested Bucket is image-demo and located in China East 1 \(Hangzhou\), with the domain name oss-cn-hangzhou.aliyuncs.com, the requested image is example.jpg, and the image access style is style-example, the URL format is constructed as follows:

\]

```
http://**image-demo**.oss-cn-hangzhou.aliyuncs.com/**example.jpg**?x-oss-process=**style/style-example**
```

The URL format for HTTPS access is:

```
https://**image-demo**.oss-cn-hangzhou.aliyuncs.com/**example.jpg**?x-oss-process=**style/style-example**
```

## Access through SDK {#section_kyw_kkn_vdb .section}

Public buckets can be accessed using URLs, whereas private files are typically accessed using SDKs. Because in Image Service, URLs are accessed with standard GET operations, only the process parameter needs to be added to the Get Object.

The Python SDK is used as an example:

```
bucket = oss2. Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
key = 'example.jpg'
new_pic = 'new-example.jpg'
process = "image/resize,m_fixed,w_100,h_100" //Scale down the image based on the target width and height
bucket.get_object_to_file(key, new_pic, process=process)
```

For more information about Image Service used for OSS SDKs, see Image Processing in the SDK documentation. The following table lists links of image processing used for some SDKs.

|SDK|Image processing document|Example|
|---|-------------------------|-------|
|Java SDK| |[ImageSample.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/ImageSample.java)|
|Python SDK| |[image.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/image.py)|
|C\# SDK| |[ImageProcessSample.cs](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/ImageProcessSample.cs)|
|PHP SDK| |[Image.php](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/samples/Image.php)|
|JS SDK| |[object.test.js](https://github.com/ali-sdk/ali-oss/blob/master/test/node/object.test.js)|
|C SDK| |[oss\_image\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_image_sample.c)|

## Image processing restrictions {#section_vsm_nln_vdb .section}

-   The supported formats include JPG, PNG, BMP, GIF, WEBP, and TIFF.
-   When the width or height of a thumbnail is specified, the image is scaled by a single side by default in the case of proportional scaling. With fixed width and height, the image is scaled down by assuming equal width and height.
-   The scaled image size is restricted. The product of the width and height of the target thumbnail cannot exceed 4096 x 4096, and the length of a single side cannot exceed 4096 x 4.
-   When resize is called, the image cannot be enlarged by default. That is, if the requested image is larger than the source image, the source image is returned. If you want to enlarge the image, add the parameter `limit_0`.
-   Currently, GIF and WEBP images can be processed once at a time to reduce resource consumption. For example, you cannot crop a GIF or WEBP image immediately after resizing it.

