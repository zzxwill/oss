# Format conversion {#concept_mf3_md5_vdb .concept}

You can convert an image to different formats, such as JPG, PNG, BMP, WebP, and GIF. By default, no format is specified and images are returned in the source format.

## Parameter {#section_zr3_f25_vdb .section}

|Name|Description|
|----|-----------|
|jpg|Saves the source image in JPG format. If the source image is in PNG, WebP, or BMP format that supports transparent channels, OSS fills the transparent section in white by default.|
|png|Saves the source image in PNG format.|
|webp|Saves the source image in WebP format.|
|bmp|Saves the source image in BMP format.|
|gif|Saves the source image in GIF format. If the source image is not in GIF format, it is saved in the source format.|
|tiff|Saves the source image in TIFF format.|

## Precautions {#section_sx3_l25_vdb .section}

-   For a standard image resize action, we recommend that you add the format parameter to the end of the image processing parameter string, such as image/resize,w\_100/format,jpg.
-   For a resize image action that includes adding a watermark to the image, we recommend that you add the format parameter next to the resize parameter, for example: image/reisze,w\_100/format,jpg/watermark,....
-   When an image is saved in JPG format, it is saved in baseline JPEG format by default. To save it in progressive JPEG format, you can set the interlace parameter. For more information, see [Gradual display](reseller.en-US/Data Processing/Image Processing/Convert formats/Gradual display.md#).

## Example {#section_jcd_425_vdb .section}

-   Save a PNG image in JPG format.

    Request URL:[http://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/format,jpg](http://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/format,jpg)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4784/15475317872554_en-US.jpg)

-   Save a PNG image in JPG format that supports progressive JPEG display.

    Request URL: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/interlace,1/format,jpg](http://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/interlace,1/format,jpg)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4784/15475317872555_en-US.jpg)

-   Save a GIF image in JPEG format.

    Request URL: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/format,jpg](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/format,jpg)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4784/15475317882556_en-US.jpg)

    Scale down the image to 200 in width.

    Request URL: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/resize,w\_200/format,gif](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/resize,w_200/format,gif)

    ![](images/2558_en-US.gif)

-   Save a GIF image in WebP format.

    Request URL: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/resize,w\_200/format,webp](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/resize,w_200/format,webp)

    ![](images/2559_en-US.webp)


