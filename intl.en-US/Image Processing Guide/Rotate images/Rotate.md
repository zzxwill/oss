# Rotate {#concept_yvv_25t_vdb .concept}

Images can be rotated in clockwise direction.

## Parameters {#section_qkk_g5t_vdb .section}

Operation name: `rotate`

|Parameter|Description|Value range|
|---------|-----------|-----------|
|value|Degrees of clockwise rotation|\[0, 360\] The default value is 0, indicating no rotation.|

## Caveats {#section_j4h_l5t_vdb .section}

-   The rotated image may become larger.
-   The size of the image to be rotated is limited. The image width or height cannot exceed 4,096.

## Example {#section_kpp_m5t_vdb .section}

-   Rotate an image 90 degrees clockwise.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/rotate,90](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/rotate,90)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4777/2524_en-US.jpg)

-   Scale down an image to 200x200 \(WxH\) and then rotate it 90 degrees clockwise.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_200,h\_200/rotate,90](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_200,h_200/rotate,90)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4777/2525_en-US.jpg)


