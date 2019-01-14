# Resize images {#concept_hxj_c4n_vdb .concept}

Generate a thumbnail of the image as required or make the specified scaling.

**Note:** The supported formats include jpg, png, bmp, gif, webp, and tiff.

## Parameters {#section_vyx_j4n_vdb .section}

Operation name: `resize`

-   Scale up and down with specified width and height

    |Name|Description|Value range|
    |----|-----------|-----------|
    |m|Specify the scaling mode:     -   lfit: proportional scaling. It refers to the maximum image that is limited in the rectangle of the specified w and h.
    -   mfit: proportional scaling. It refers to the minimum image extending out of the rectangle of the specified w and h.
    -   fill: fixed width and height. It refers to the cropped and centered minimum image extending out of the rectangle of the specified w and h.
    -   pad: fixed width and height, scaling down and filling.
    -   fixed: fixed width and height, enforced scaling down.
|\[lfit, mfit, fill, pad, fixed\], the default value is lfit.|
    |w|Specify the target width.|1-4096|
    |h|Specify the target height.|1-4096|
    |l|Specify the longer side of the target.|1-4096|
    |s|Specify the shorter side of the target.|1-4096|
    |limit|Specify whether to process the target thumbnail when it is larger than the original image. 1 indicates not to process, and 0 indicates to process.|0/1. The default value is 1|
    |color|When you set the scaling mode as pad \(scaling down and filling\), you can select the filling color \(The default is white\). Filling format of parameters: use hexadecimal color codes, for example 00FF00 \(green\).|\[000000-FFFFFF\]|

-   Proportional scaling
-   |Name|Description|Value range|
|----|-----------|-----------|
|p|Percentage. If it is smaller than 100, it means to scale down; if it is bigger than 100, it means to scale up.|1-1000|


## Note {#section_vt5_1qn_vdb .section}

-   For the original image:
    -   Formats supported: jpg, png, bmp, gif, webp, and tiff.
    -   File size cannot exceed 20 MB.
    -   When using the image rotation, the width or height of the image cannot exceed 4096.
    -   The size of a single side cannot exceed 30,000.
-   For the thumbnail: The scaled image size is restricted. The product of the width and height of the target thumbnail cannot exceed 4096 x 4096, and the length of a single side cannot exceed 4096 x 4.
-   When the width or height of a thumbnail is specified, the image is scaled by a single side by default in the case of proportional scaling. With fixed width and height, the image is scaled down by assuming equal width and height.
-   When only the width or height of a thumbnail is specified, the image is returned in the same format as the original image. If you want to save the image into other formats, see [Quality Transformation](reseller.en-US/Data Processing/Image Processing/Convert formats/Quality Transformation.md#) and [Format conversion](reseller.en-US/Data Processing/Image Processing/Convert formats/Format conversion.md#).
-   When resize is called, the image cannot be enlarged by default. That is, if the requested image is larger than the original image, the original image is returned. If you want to enlarge the image, add the parameter`limit,0` to be called \(for example: `https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_500,limit_0`\)

## Example {#section_l4x_pqn_vdb .section}

**Scaling-down by a single side \(by width and height\)**

-   Scale down an image to 100 in height, and the width is adjusted proportionally.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,h\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,h_100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4769/15474295232414_en-US.jpg)


**Scaling-down by a single side \(by the longer side and shorter side\)**

-   Limit the longer side of an image to 100, and the shorter side is adjusted proportionally.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,l\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,l_100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4769/15474295242415_en-US.jpg)


**Scaling-down based on target width or height**

-   Scale down an image to 100 x 100 \(w x h\).

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_fixed,h\_100,w\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_fixed,h_100,w_100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4769/15474295242416_en-US.jpg)


**Proportional scaling, restricted in a rectangle frame**

-   Scale down an image by the longer side to 100 x 100 \(w x h\).

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_lfit,h\_100,w\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_lfit,h_100,w_100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4769/15474295242418_en-US.png)

-   Scale down an image by the longer side to 100 x 100 \(w x h\) and save it as png.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_lfit,h\_100,w\_100/format,png](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_lfit,h_100,w_100/format,png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4769/15474295242419_en-US.png)


**Proportional scaling, restricted out of a rectangle frame**

-   Scale down an image by the shorter side to 100 x 100 \(w x h\)

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_mfit,h\_100,w\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_mfit,h_100,w_100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4769/15474295242420_en-US.jpg)


**Fixed width and height, automatic cropping**

-   Automatically crop an image to 100 x 100 \(w x h\)

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_fill,h\_100,w\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_fill,h_100,w_100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4769/15474295242425_en-US.jpg)


**Fixed width and height, scaling down and filling**

-   Scale down an image by the shorter side to 100 x 100, and then fill the remaining area with a solid color.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_pad,h\_100,w\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_pad,h_100,w_100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4769/15474295242421_en-US.jpg)

-   Scale down an image by the shorter side to 100 x 100, and then fill the remaining area with red.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_pad,h\_100,w\_100,color\_FF0000](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_pad,h_100,w_100,color_FF0000)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4769/15474295242422_en-US.jpg)

-   Scale down an image to 1/2 of the original size.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,p\_50](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,p_50)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4769/15474295242423_en-US.jpg)


