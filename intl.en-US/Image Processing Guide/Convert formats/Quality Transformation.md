# Quality Transformation {#concept_exc_qp5_vdb .concept}

If the image is saved as JPG or webp, the quality transformation can be supported.

## Parameter {#section_mps_tp5_vdb .section}

Operation name: `quality`

|Name|Description|Value range|
|----|-----------|-----------|
|q| Determine the relative quality of the picture and compress the original graph according to Q. If the original graph has a quality of 100%, use 90 Q to get an image of quality of 90% If the original image quality is 80%, the use of 90 Q will get a picture of the quality of 72.

 The concept of relative compression can only be used on images in the original image that are in JPG format. If the original diagram is webp, then the relative mass is equivalent to absolute mass.

 |1-100|
|Q| To determine the absolute quality of the picture, press the original graph quality to Q %, if the original graph quality is less than the specified number, not compressed. If the original graph quality is 100%, the use of "90q" will get a picture of the quality of 90; if the original graph quality is 95%, the use of "90 Q" will also get a picture of the quality of 90; if the original graph quality is 80%, using "90q" does not compress and returns the original graph of quality 80.

 It can be used only on the SAVE as JPG/webp effects, and the other formats have no effect. If both Q and q are specified, press Q to process.

 |1-100|

## Note: {#section_uyz_pt5_vdb .section}

If you don't fill in Q parameter or q  parameters, it may cause the image to take up more space. If you really want to get a picture of fixed quality, use the Q parameter.

## Examples {#section_bcd_xt5_vdb .section}

-   The original graph is reduced to a JPG graph of 80% relative to the original graph quality.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_100,h\_100/quality,q\_80](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100,h_100/quality,q_80)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4786/2629_en-US.jpg)

-   The original graph is reduced to a JPG graph with an absolute quality of 80.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_100,h\_100/quality,Q\_80](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100,h_100/quality,Q_80)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4786/2630_en-US.jpg)


