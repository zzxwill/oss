# Video intercept {#concept_kz1_cwc_wdb .concept}

The Image Service not only processes the existing image content but also captures the image at a specified point of the video to complete the video frame capturing.

## Parameters {#section_oqh_dwc_wdb .section}

Operation type: `video`

Operation name: `snapshot`

|Parameter|Description|Value range|
|---------|-----------|-----------|
|t|Screenshot time|Unit: ms. \[0, video duration\]|
|w|Screenshot width. If it is specified as 0, the value|is automatically calculated. Pixel value: \[0, video width\]|
|h|Screenshot height. If it is specified as 0, the value is automatically calculated. If both w and h are 0, the video is outputted in the original width and height.|Pixel value: \[0, video width\]|
|m|Screenshot mode. If not specified, use the default mode. The screenshot is captured accurately at a specified time. If it is set as fast, the most recent key frame before the specific time is captured.|Enumeration value: fast|
|f|Output image format|Enumeration value: jpg and png|

## Example {#section_aql_kwc_wdb .section}

-   Find the video content at 7s, and set the output type as jpg.

    [http://a-image-demo.oss-cn-qingdao.aliyuncs.com/demo.mp4?x-oss-process=video/snapshot,t\_7000,f\_jpg,w\_800,h\_600,m\_fast](http://a-image-demo.oss-cn-qingdao.aliyuncs.com/demo.mp4?x-oss-process=video/snapshot,t_7000,f_jpg,w_800,h_600,m_fast)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4801/2927_en-US.jpg)

-   Find the video content at 50s, and set the output type as jpg. Accurate to the specific time.

    [http://a-image-demo.oss-cn-qingdao.aliyuncs.com/demo.mp4?x-oss-process=video/snapshot,t\_50000,f\_jpg,w\_800,h\_600](http://a-image-demo.oss-cn-qingdao.aliyuncs.com/demo.mp4?x-oss-process=video/snapshot,t_50000,f_jpg,w_800,h_600)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4801/2928_en-US.jpg)


