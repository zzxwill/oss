# Rounded rectangle {#concept_xts_dss_vdb .concept}

This feature allows you to save an image in a rounded oblong shape and specify the rounded corner size.

## Parameters {#section_gcj_rts_vdb .section}

This table provides the description and values for parameters for the `rounded-corners` operation.

|Parameter|Description|Value|
|---------|-----------|-----|
|r|Radius of the cropped rounded corner of an image.|\[1, 4096\] The radius of the largest rounded corner cannot exceed half of the shorter side of the source image.|

## Caveats {#section_zvn_d5s_vdb .section}

-   If the final format of the image is PNG, WebP, or BMP, and supports transparent channels, the area of the image outside the circular area is transparent. If the final format of the image is JPG,  the area of the image outside the circular area is white. The PNG format is recommended.
-   If the specified radius is greater than the radius of the largest incircle of the source image, the circle is still the largest incircle of the source image.

## Example {#section_knv_g5s_vdb .section}

-   Crop an image with the radius of the cropped rounded corner being 30, and save the cropped image as JPG.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/rounded-corners,r\_30](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/rounded-corners,r_30)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4774/2502_en-US.jpg)

-   Crop an image to 100x100 in size, and save the image as PNG with the radius of the rounded corner being 10.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,w\_100,h\_100/rounded-corners,r\_10/format,png](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,w_100,h_100/rounded-corners,r_10/format,png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4774/2503_en-US.png)


