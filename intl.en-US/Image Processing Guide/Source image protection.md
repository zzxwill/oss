# Source image protection {#concept_dsx_1rc_wdb .concept}

To avoid image piracy risks, the exposure to image URLs must be restricted so that only thumbnailed or watermarked images can be obtained. To do this, you can enable source image protection.

## Rule description {#section_gxd_crc_wdb .section}

**After enabling the source image protection, you cannot access images in the following two ways:**

-   Access directly with an OSS address: `http://bucket.<endpoint>/object`.
-   Request thumbnails with processing parameters: `http://bucket.<endpoint>/object?x-oss-process=image/action,parame_value`

**You can only access images in style mode:**

-   Access through URL parameters`http://bucket.<endpoint>/object?x-oss-process=style/<StyleName>`
-   Access through separators`http://bucket.<endpoint>/object<separator><StyleName>`

**Note:** 

-   The preceding rules only apply to anonymous accesses to public-read files. After enabling the source image protection, you can obtain source images using a signature-based method.
-   The source image protection is designed for protecting image files, and the suffixes of the image files to be protected must be set. For example, if .jpg files are set for source image protection, you can still directly access the source images of .png files.

You can configure the access rules in the **Image Processing** module of the bucket in the console. **Access rules**

## Configure access rules {#section_pfs_4rc_wdb .section}

1.  In the left-side bucket list of the [OSS console](https://oss.console.aliyun.com/overview), click the bucket for which you want to set the source image protection.
2.  Click the **Image Processing** tab to locate the Access Settings button. See the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4793/15439766502907_en-US.png)

3.  Click **Access Settings** to open the Access Settings dialog box, as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4793/15439766502908_en-US.png)

    In the Settings dialog box, perform the following settings:

    -   Enable source image protection: After enabling the source image protection, you can only access the image file by passing in the stylename or using a signature-based method. Direct accesses to the OSS source file or accesses by passing in image parameters and modifying the image style are not allowed.
    -   Set the suffixes of the image files for source image protection.
    -   Customize separators.
4.  Once you set the needed options, click **OK** to finish setting the source image protection.

