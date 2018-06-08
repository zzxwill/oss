# Sharpen {#concept_cyw_zzt_vdb .concept}

This feature allows you to sharpen a processed image to make it clearer.

## Parameters {#section_c4s_b15_vdb .section}

This table provides a description and value range of the value parameter for the `sharpen` operation.

|Parameter|Description|Value range|
|---------|-----------|-----------|
|value|Sharpens an image. The parameter value indicates the degree of sharpness. The greater the value, the clearer the image.|\[50, 399\] We recommend that you set this parameter to 100 for optimal effect.|

## Example {#section_bhr_215_vdb .section}

-   Sharpen an image, with the parameter set to 100. To see this example, follow the following link:

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/sharpen,100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/sharpen,100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4782/2537_en-US.jpg)

-   Scale down an image to 200 in width and sharpen the image with the parameter set to 100. To see this example, follow the following link:

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_200/sharpen,100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_200/sharpen,100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4782/2539_en-US.jpg)


