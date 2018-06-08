# Blur {#concept_bwn_mvt_vdb .concept}

This feature allows you to apply the blur effect to an image.

## Parameters {#section_sl2_pvt_vdb .section}

This table provides the description and values for parameters when applying the `blur` operation.

|Parameter|Description|Value|
|---------|-----------|-----|
|r|Blur radius|\[1,50\] The greater the value of r, the blurrier the image.|
|s|Standard deviation of a normal distribution|\[1,50\] The greater the value of s, the blurrier the image.|

## Example {#section_rq1_bwt_vdb .section}

-   Blur an image, with the radius being 3 and standard deviation being 2. To see this example, follow the following link:

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/blur,r\_3,s\_2](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/blur,r_3,s_2)

    ![](images/2526_en-US.jpg)

-   Scale down an image to 200 in width, and blur it with the radius being 3 and standard deviation being 2. To see this example, follow the following link:

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_200/blur,r\_3,s\_2](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_200/blur,r_3,s_2)

    ![](images/2527_en-US.jpg)


