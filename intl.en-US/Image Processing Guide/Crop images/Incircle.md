# Incircle {#concept_n5s_3ms_vdb .concept}

This feature allows you to save an image in a circular shape. If the final format of the image is PNG, WebP, or BMP supporting transparent channels, the area of the image outside the circular area is transparent. If the final format of the image is JPG, the area of the image outside the circular area is white.

## Parameters {#section_t5p_tms_vdb .section}

This table provides the description and values for parameters for the`circle`operation.

|Parameter|Description|Value|
|---------|-----------|-----|
|r|Radius of the circular area of the image|The radius r cannot exceed half of the shorter side of the source image. If the radius r exceeds half of the shorter side of the source image, the circle is still the largest incircle of the source image.|

## Caveats {#section_kgj_yms_vdb .section}

-   If the final format of the image is PNG, WebP, or BMP supporting transparent channels, the area of the image outside the circular area is transparent. If the final format of the image is JPG, the area of the image outside the circular area is white.  The PNG format is recommended.
-   If the specified radius is greater than the radius of the largest incircle of the source image, the circle is still the largest incircle of the source image.

## Example {#section_wbp_bns_vdb .section}

-   Crop an image with a crop radius of 100 and keep the original circular size.  If the image is saved in JPEG format, the area of the image outside the circular area is white.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/circle,r\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/circle,r_100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4771/2477_en-US.jpg)

-   Crop an image with a crop radius of 100 and save the circle as the smallest square that can enclose the circle. If the image is saved in PNG format, the area of the image outside the circular area is transparent.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/circle,r\_100/format,png](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/circle,r_100/format,png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4771/2478_en-US.png)


