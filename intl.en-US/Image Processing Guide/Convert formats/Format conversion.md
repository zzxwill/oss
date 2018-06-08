# Format conversion {#concept_mf3_md5_vdb .concept}

You can convert an image to different formats, such as JPG, PNG, BMP, WebP, and GIF. By default, no format is specified and images are returned in the source format.

## Parameters {#section_zr3_f25_vdb .section}

|Name|Description|
|----|-----------|
|jpg|Saves the source image as JPG. If the source image is in PNG, WebP, or BMP format supporting transparent channels, the system fills the transparent section in white by default.|
|png|Saves the source image as PNG.|
|webp|Saves the source image as WebP.|
|bmp|Saves the source image as BMP.|
|gif|Saves the source image in GIF format as GIF. If the source image is in another format, it is saved in the source format.|
|src|Returns the source image in the source format. If the source image is in GIF format, the first GIF frame is returned and saved as JPG instead of GIF. If you want to save it as GIF, add the 1an parameter.|

## Caveats {#section_sx3_l25_vdb .section}

When an image is saved as JPG, it is saved as baseline JPEG by default. To save it as progressive JPEG, you can set the interlace parameter. For more information, see [Gradual display](intl.en-US/Image Processing Guide/Convert formats/Gradual display.md#). 

## Example {#section_jcd_425_vdb .section}

-   Save a PNG image as JPG.

    Request URL:[http://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/format,jpg](http://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/format,jpg)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4784/2554_en-US.jpg)

-   Save a PNG image as JPG with progressive JPEG display.

    Request URL: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/interlace,1/format,jpg](http://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/interlace,1/format,jpg)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4784/2555_en-US.jpg)

-   Save a GIF image as JPEG.

    Request URL: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/format,jpg](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/format,jpg)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4784/2556_en-US.jpg)

    Scale down the image to 200 in width.

    Request URL: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/resize,w\_200/format,gif](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/resize,w_200/format,gif)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4784/2558_en-US.gif)

-   Save a GIF image as WEBP.

    Request URL: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/resize,w\_200/format,webp](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/resize,w_200/format,webp)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4784/2559_en-US.webp)


