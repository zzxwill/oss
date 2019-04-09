# Capture video frames {#concept_kz1_cwc_wdb .concept}

Alibaba Cloud OSS Image Processing \(IMG\) can not only process existing images but also capture the frame at a specified time in a video file as an image.

**Note:** 

-   Currently, OSS can only capture frames from video files in the H.264 format.
-   Captured images are not stored by default. You must manually download the captured images to the local device.

## Parameters {#section_oqh_dwc_wdb .section}

Operation type: `video`

Operation name: `snapshot`

|Parameter|Description|Value range|
|---------|-----------|-----------|
|t|Specifies the time when the image needs to be captured.| Unit: ms

 Range: \[0, video duration\]

 |
|w|Specifies the width of the captured image. If this parameter is set to 0, the width of the image is automatically calculated.|Pixel value: \[0, video width\]|
|h|Specifies the height of the captured image. If this parameter is set to 0, the height of the image is automatically calculated. If w and h are both set to 0, the width and height of the captured image are the same as those of the video file.|Pixel value: \[0, video height\]|
|m|Specifies the image capturing mode. If this parameter is not specified, the image is captured in the default mode, that is, the image at the specified time in the video is captured. If it is set as fast, the most recent key frame before the specified time is captured.|Enumerated value: fast|
|f|Specifies the format of the captured image.|Enumerated value: jpg and png|

## Example {#section_aql_kwc_wdb .section}

-   Capture the frame at the 7th second and output it as an image in jpg format.

    [http://a-image-demo.oss-cn-qingdao.aliyuncs.com/demo.mp4?x-oss-process=video/snapshot,t\_7000,f\_jpg,w\_800,h\_600,m\_fast](http://a-image-demo.oss-cn-qingdao.aliyuncs.com/demo.mp4?x-oss-process=video/snapshot,t_7000,f_jpg,w_800,h_600,m_fast)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4801/15547999552927_en-US.jpg)

-   Capture the frame at the 50th second accurately and output it as an image in jpg format.

    [http://a-image-demo.oss-cn-qingdao.aliyuncs.com/demo.mp4?x-oss-process=video/snapshot,t\_50000,f\_jpg,w\_800,h\_600](http://a-image-demo.oss-cn-qingdao.aliyuncs.com/demo.mp4?x-oss-process=video/snapshot,t_50000,f_jpg,w_800,h_600)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4801/15547999572928_en-US.jpg)


