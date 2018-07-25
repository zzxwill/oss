# Adaptive orientation {#concept_ugq_tvs_vdb .concept}

The photos taken by some mobile phones may contain rotation parameters \(saved as EXIF data of the photos\). You can configure whether to rotate such photos. By default, adaptive orientation is configured.

## Parameters {#section_y3b_vvs_vdb .section}

Operation name: `auto-orient`

|Parameter|Description|Value range|
|---------|-----------|-----------|
|value| Indicates whether to perform auto rotation.

 0 indicates that the orientation of the source image is retained without auto rotation.

 1 indicates rotating and then scaling down the image.

 |0 and 1|

## Caveats {#section_c2t_cws_vdb .section}

-   To apply adaptive orientation, make sure that the width and height of the source image are smaller than 4,096.
-   If the source image does not contain rotation parameters, setting the parameter of auto-orient for the image does not affect the image.

## Example {#section_vlk_2ws_vdb .section}

-   Scale down an image to 100 in width without auto rotation

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/resize,w\_100/auto-orient,0](http://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/resize,w_100/auto-orient,0)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4776/2507_en-US.jpg)

-   Scale down an image to 100 in width, and auto-rotate the image by setting the value parameter to 1.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/resize,w\_100/auto-orient,1](http://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/resize,w_100/auto-orient,1)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4776/2508_en-US.jpg)

    The target image size is 100x127 \(WxH\).


