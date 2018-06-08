# Brightness {#concept_mv4_fxt_vdb .concept}

This feature allows you to adjust the brightness of a processed image.

## Parameters {#section_ccf_jxt_vdb .section}

This table provides a description and value range of the parameters for the `bright` operation.

|Parameter|Description|Value range|
|---------|-----------|-----------|
|value|Brightness adjustment. 0 indicates the original brightness. A value smaller than 0 indicates a brightness lower than the original brightness, and a value greater than 0 indicates a brightness higher than the original brightness.|\[-100, 100\]|

## Example {#section_skg_txt_vdb .section}

-   Adjust only the brightness of the source image. To see this example, follow the following link:

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/bright,50](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/bright,50)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4780/2529_en-US.jpg)

-   Scale down an image to 200 in width and adjust its brightness. To see this example, follow the following link:

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_200/bright,50](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_200/bright,50)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4780/2530_en-US.jpg)


