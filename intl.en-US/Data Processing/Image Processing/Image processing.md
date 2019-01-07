# Image processing {#concept_m4f_dcn_vdb .concept}

Alibaba Cloud OSS Image Processing \(IMG\) is an image processing service that features massive capacity, high security, low costs, and high reliability. By uploading and storing original images in OSS, you can process images anytime, anywhere, on any Internet device through a simple RESTful API. IMG offers image processing APIs. To upload images, use the OSS upload API. IMG is a great solution for you to build image-related services.

**Note:** 

IMG is activated automatically when you activate OSS.

## Basic features {#section_jky_lcn_vdb .section}

IMG provides the following features:

-   Retrieving image information
-   Converting image formats
-   Scaling, cropping, and rotating images
-   Adding images, texts, and text-and-image watermarks to images
-   Customizing image processing styles
-   Calling multiple image processing features in a set sequence through pipelines

## Previous versions {#section_irr_ncn_vdb .section}

IMG now has two API versions.

**Note:** This article introduces the features of the new version. Features of the old APIs will not be updated. For compatibility details, see [FAQ on using old and new versions of APIs and domain names](reseller.en-US/Data Processing/Image Processing/FAQ on using old and new versions of APIs and domain names.md#).

## Quick start {#section_asc_qcn_vdb .section}

**Create an image style**

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  Click your bucket name to go to the **Overview** page of the bucket.
3.  On the **Overview** page, click **Image Processing**, and then click **Create Style**.
4.  Create an image style in the Image Style page.

    Details about the Image Style page:

    -   **Style Name**: Name of the image style to create. We recommend you give the style a meaningful name so that you can remember it easily, such as XX watermark image rotation.
        -   The length of the name must be within 1-64 characters.
        -   A name can only include numbers, letters, underscores \( \_ \), short crosslines \(-\), and the decimal point \(.\).
    -   **Editing Type**: You can select "Basic editing" to edit the image style with graphical operations. You can also select "Advanced editing" to edit the image style using an SDK or parameters.
    -   **Resize Mode**: Set the scaling mode for the image.

        **Note:** The "long side" refers the side with a bigger source size to target size ratio. The same applies to the "short side". For example, for an original image that is scaled from 400x200 to 800x100, the original-to-target ratios are 0.5 \(400/800\) and 2 \(200/100\). Because 0.5 is less than 2, the 200 side is the longer side, and the 400 side the shorter one.

    -   **Adaptive Orientation**: Set the adaptive orientation for the image.

        It is recommended that you enabled it by default. An image is firstly rotated and then resized based on the EXIF information.

    -   Save format: the original format, JPG, PNG, webp, and BMP formats are available for selection.
    -   **Image Sharpening**: Set whether the image needs to be sharpened.
    -   **Image Quality**: Set the image quality.
    -   **Watermark**: Set the image watermark mode.
5.  Edit the image style and click **OK** to save the style.

After creating the new image style, you can apply it to your images through OSS.

**Apply an image style**

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  Click your bucket name to go to the **Overview** page of the bucket.
3.  On the Overview page, click **Files** to select an existing image or upload a new image to open the Preview page.

    For new image uploading, see [Upload objects](../../../../../reseller.en-US/Console User Guide/Manage objects/Upload objects.md#). 

4.  Select a picture style from the **Image Style** drop-down list.

    You can view the processed image in the preview window immediately. A public network access address with the image style is generated at the same time. You only need to click **Copy File URL** to get the access address to the file.


