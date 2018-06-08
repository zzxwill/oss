# Gradual display {#concept_avy_pf5_vdb .concept}

There are two ways to present a picture in JPG format:

-   Top-down Scanning
-   Fuzzy first and then gradually clear \(when the network environment is poor\)

The default is saved as the first, if you want to specify a presentation that is vague and clear, use the progressive display parameters.

## Parameter {#section_dyc_3g5_vdb .section}

Operation name: `interlace`

|Name|Description|Value range|
|----|-----------|-----------|
|\[value\]|1 For JPG format saved as a progressive display 0 for a normal JPG format|\[0, 1\]|

**Note:** 

This parameter only makes sense if the effect graph is in JPG format.

## Examples {#section_vlm_4j5_vdb .section}

-   Saves the photos in PNG format to a JPG format that is displayed incrementally.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/format,jpg/interlace,1](http://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/format,jpg/interlace,1)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4785/2592_en-US.jpg)

-   The image is reduced to a width of 200, and saved in a gradually displayed JPG format.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/format,jpg/interlace,1](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/format,jpg/interlace,1)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4785/2593_en-US.jpg)


