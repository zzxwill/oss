# Contrast {#concept_dpw_4yt_vdb .concept}

This feature allows you to adjust the contrast of a processed image.

## Parameters {#section_htf_qyt_vdb .section}

This table provides a description and value range of the value parameter for the `contrast` operation.

|Parameter|Description|Value range|
|---------|-----------|-----------|
|value|Contrast adjustment. 0 indicates the original contrast. A value smaller than 0 indicates a contrast lower than the original contrast, and a value greater than 0 indicates a contrast higher than the original contrast.|\[-100, 100\]|

## Example {#section_x1x_2zt_vdb .section}

-   Adjust only the contrast of the source image. To see this example, follow the following link:

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/contrast,-50](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/contrast,-50)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4781/2532_en-US.jpg)

-   Scale down an image to 200 in width and adjust its contrast. To see this example, follow the following link:

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_200/contrast,-50](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_200/contrast,-50)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4781/2534_en-US.jpg)


