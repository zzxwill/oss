# Crop {#concept_bbn_14s_vdb .concept}

This feature allows you to crop images by specifying the starting point of where you want to crop, and the width and height of the cropped area.

## Parameters {#section_kzm_c4s_vdb .section}

This table provides the description and values of the parameters for the `crop`operation.

|Parameter|Description|Value range|
|---------|-----------|-----------|
|W|Width of the cropped area|\[0-image width\]|
|h|Height of the cropped area|\[0-image height\]|
|x|X-axis of the crop starting point \(the origin is located in the upper-left corner by default\)|\[0-image border\]|
|y|Y-axis of the crop starting point \(the origin is located in the upper-left corner by default\)|\[0-image border\]|
|g|Location of the origin for cropping. The origin is located in the upper-left corner of any of nine fixed cells.|\[nw,north,ne,west,center,east,ne\]|

Schematic view of the g parameter indicating the origin for cropping:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4772/2485_en-US.png)

## Caveats {#section_bhy_2ps_vdb .section}

-   If the specified starting X-axis and Y-axis values exceed the source image, a BadRequest error is returned.  You must specify different values to crop the image.
-   If the width and height specified from the starting point exceed the source image size, the source image is cropped to its edges.

## Example {#section_fst_gps_vdb .section}

-   Crop an image from the starting point \(100, 50\) to the edges.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x\_100,y\_50](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x_100,y_50)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4772/2486_en-US.jpg)

-   Crop an area of 100x100 from the starting point \(100, 50\).

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x\_100,y\_50,w\_100,h\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x_100,y_50,w_100,h_100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4772/2487_en-US.jpg)

-   Crop an area of 200x200 in the lower-right corner of the source image.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x\_0,y\_0,w\_200,h\_200,g\_se](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x_0,y_0,w_200,h_200,g_se)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4772/2488_en-US.jpg)

-   Crop an area of 200x200 in the lower-right corner of an image and stretch the cropped area downward by 10x10.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x\_10,y\_10,w\_200,h\_200,g\_se](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x_10,y_10,w_200,h_200,g_se)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4772/2491_en-US.jpg)


