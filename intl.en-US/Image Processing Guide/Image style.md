# Image style {#concept_mr2_x3c_wdb .concept}

Adding all the changes to the image after the URL makes the URL too long and inconvenient for management and reading. IMG allows you to save common operations as an alias, that is, a style.  With the style, a complicated operation can be performed through a short URL.

Multiple styles \(50 at most\) are grouped under a bucket. Each style is effective only within the bucket.

## Style access rules {#section_uvy_pjc_wdb .section}

**URL parameters**

```
<File URL>? x-oss-process=style/<StyleName>
```

Example:

`bucket.aliyuncs.com/sample.jpg? x-oss-process=style/stylename`

This is the default style access method supported by IMG.

**Separators**

```
<File URL><Separator><StyleName>
```

Example:

`bucket.aliyuncs.com/sample.jpg@! stylename`

`@!` is the style separator. IMG regards the content after the separator in a URL as the style name. This is an optional method provided by IMG. You can also set separators in the console.  Separators such as `-`, `_`, `/`, and `!` are also supported.

-   StyleNameindicates the name of a style.
-   Style creations, deletions, and modifications are all performed in the front-end console.
-   When the requested style does not exist in the specified bucket, 

**Note:** the system returns the “NotSuchStyle” error.

## Set separators {#section_djg_j4c_wdb .section}

1.  In the left-side bucket list of the [OSS console](https://oss.console.aliyun.com/overview), click the bucket to which you want to set separators.
2.  Click the **Image Processing** tab, and then click **Access Settings** . As shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4792/2882_en-US.png)

3.  In the **Access Settings** dialog box, set  

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4792/2883_en-US.png)

    the following parameters:

    -   Source Image Protection: After enabling the original image protection, you can only access the image file by passing in the StyleName or using a signature-based method. Direct accesses to the OSS original file or accesses by passing in image parameters and modifying the image style are not allowed.
    -   Customize separator
    Click **OK**.


## Example {#section_unh_hqc_wdb .section}

In this example, a style is created in the bucket image-demo.

|Style name|Style content|
|----------|-------------|
|panda\_sytle|image/resize,m\_fill,w\_300,h\_300,limit\_0/auto-orient,0/quality,q\_90/watermark,image\_cGFuZGEucG5n,t\_61,g\_se,y\_10,x\_10|

-   Access through parameters

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_fill,w\_300,h\_300,limit\_0/auto-orient,0/quality,q\_90/watermark,image\_cGFuZGEucG5n,t\_61,g\_se,y\_10,x\_10](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_fill,w_300,h_300,limit_0/auto-orient,0/quality,q_90/watermark,image_cGFuZGEucG5n,t_61,g_se,y_10,x_10)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4792/2884_en-US.jpg)

-   Access through URL parameters in style mode

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/panda\_style](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/panda_style)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4792/2885_en-US.jpg)

-   Access through style separators in style mode

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg@!panda\_style](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg@!panda_style)

    ![](http://icms-static-translation.oss-cn-hangzhou.aliyuncs.com/SP_21/DNOSS11827905/images/2886_zh-CN.jpg%40%21panda_style?Expires=1528618442&OSSAccessKeyId=LTAIJfoPL6wmrirR&Signature=1WiITW7CdWKdx%2BDbH5qy2vmRpeU%3D)


These three methods bring the same result.

