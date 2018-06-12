# Add watermarks {#concept_hrt_sv5_vdb .concept}

This feature allows you to add an image or text as a watermark to another image.

## Parameters {#section_bgt_vv5_vdb .section}

This table provides a description of the basic parameters and their values, which can be used with the `watermark` operation.

**Basic parameters**

|Name|Description|Parameter type|
|----|-----------|--------------|
|t| It indicates the transparency. This parameter makes the added image watermark or text watermark transparent

 Default value: 100 \(in the unit of %\), indicating no transparency; value range: \[0–100\] 

 |Optional|
|g| It indicates the position of a watermark on the target image. The position is shown in the following figure.

 Value range: \[nw,north,ne,west,center,east,sw,south,se\]

 |Optional|
|x| It indicates the horizontal margin, that is, the horizontal distance between the watermark and the image edge. This parameter is meaningful only when the watermark is in the upper left, middle left, lower-left corner, upper right, middle right, or lower-right corner corner of the image.  

 Default value: 10

 Value range: \[0–4,096\]

 Unit: pixel \(px\)

 |Optional|
|y|It indicates the vertical margin, that is, the vertical distance between the watermark and the image edge. This parameter is meaningful only when the watermark is in the upper left, top center, upper right, lower-left corner, bottom center, or lower-right corner corner of the image. Default value: 10 Value range: \[0–4,096\] Unit: pixel \(px\)|Optional|
|voffset|It indicates the midline vertical offset. When the watermark is in the middle left, center, or middle right of the image, you can designate the vertical offset of the watermark along the midline. Default value: 0 Value range: \[–1,000, 1,000\] Unit: pixel \(px\)|Optional|

**Note:** 

-   In addition to the position of a watermark on the image, the horizontal margin, vertical margin, and the midline vertical offset can regulate the watermark layout when the image has multiple watermarks.
-   The URL-safe Base64 encoding can be used during image processing. For more information, see RFC4648 or the URL-safe Base64 encoding section.
-   The Parameter-Position Mapping Table for the g parameter, is provided as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4787/2648_en-US.png)

**Image watermark parameters**

|Name|Description|Parameter type|
|----|-----------|--------------|
|image| It indicates the object name of an image watermark \(which must be encoded\).

 **Note:** The URL-safe base64 encoding is required: encodedObject = url\_safe\_base64\_encode\(object\). For example, if the object name is \`panda.png\`, the encoded name is \`cGFuZGEucG5n\`.

 |Required parameter|

**Watermark image preprocessing**

When a user applies a watermark, the watermark image can be preprocessed. Supported preprocessing operations include: image scaling, image cropping \(incircle not supported\), and image rotation. Additionally, another parameter is supported for the resize operation: P. P indicates the watermark image scale relative to the master image. The value range is \[1-100\], indicating the scale percentage.

**Preprocessing examples**

For example, if P\_10 is set, for a master image of 100x100, the size of the watermark is 10x10. If the same watermark processing parameters are applied to images of different sizes, the watermark image may be too large or too small. The P parameter solves this problem. Using the p parameter, IMG dynamically adjusts the size of the watermark image according to the size of the main image.

If you scale panda.png to 30% in width, then the watermark file is: \`panda.png?x-oss-process=image/resize,P\_30\` After adding URL-safe Base64 encoding this watermark file is: \`cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA\`

If the watermark is placed in the lower-right corner corner and the source image width is reduced to 400, the watermark operation is: \`watermark=1&object=cGFuZGEucG5nQDMwUA&t=90&p=9&x=10&y=10\`. This is applied to the image as follows:

[http://image-demo.img-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_400/watermark,image\_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,t\_90,g\_se,x\_10,y\_10](http://image-demo.img-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_400/watermark,image_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,t_90,g_se,x_10,y_10)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4787/2651_en-US.jpg)

If the source image is reduced to 300 in width, the watermark operation is:

[http://image-demo.img-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_300/watermark,image\_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,t\_90,g\_se,x\_10,y\_10](http://image-demo.img-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_300/watermark,image_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,t_90,g_se,x_10,y_10)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4787/2653_en-US.jpg)

**Text watermark**

|Name|Description|Parameter type|
|----|-----------|--------------|
|text| It indicates the text of the text watermark \(encode required\).

 **Note:** The URL-safe base64 encoding is required: encodeText = url\_safe\_base64\_encode\(fontText\). Maximum length: 64 characters

 |Required|
|type| It indicates the literal type of a text watermark \(encoding required\)

 **Note:** NOTE: The URL-safe base64 encoding is required: encodeText = url\_safe\_base64\_encode\(fontType\).

 Value range: See the Literal Type Encoding Table as follows. 

 Default value: wqy-zenhei \(encoded value: d3F5LXplbmhlaQ\)

 |Optional|
|color| It indicates the color of the textual content of a text watermark \(encoding required\)

 The URL-safe base64 encoding is required: EncodeFontColor = url\_safe\_base64\_encode\(fontColor\). The parameter format must be \# + six hexadecimal numbers. For example, \#000000 indicates black. "\#" indicates the prefix. Every two digits of 000000 constitute an RGB color. \#FFFFFF indicates the white color.

 Default value: \#000000 \(black\)

 |Optional|
|size| It indicates the size \(px\) of the textual content of a text watermark.

 Value range: \(0, 1,000\]

 Default value: 40

 |Optional|
|shadow| It indicates the shadow transparency of a text watermark.

 Value range: \(0, 100\]

 |Optional|
|rotate| It indicates the clockwise rotation angle of the text.

 Value range: \[0,360\]

 |Optional|
|fill| It indicates the effect of filling the image with a watermark.

 Value range: \[0,1\]. 1 indicates that the image is filled with the watermark; 0 indicates no filling effect.

 |Optional|

**Literal type encoding table**

|Parameter value|Meaning|URL-safe Base64 encoded value|Remarks|
|---------------|-------|-----------------------------|-------|
|wqy-zenhei|Wen quan yi zheng hei ti, a type of Chinese font|D3F5LXplbmhlaQ =|According to the RFC, the padding characters == can be omitted, that is, d3F5LXplbmhlaQ|
|wqy-microhei|Micro Hei font of the WenQuanYi Chinese font project|d3F5LW1pY3JvaGVp| |
|fangzhengshusong|Founder ShuSong, a Chinese Simplified font|ZmFuZ3poZW5nc2h1c29uZw==|According to the RFC, the padding characters == can be omitted, that is, ZmFuZ3poZW5nc2h1c29uZw|
|fangzhengkaiti|Founder Kai, a Chinese Simplified font|ZmFuZ3poZW5na2FpdGk=|According to the RFC, the padding character = can be omitted, that is, ZmFuZ3poZW5na2FpdGk|
|fangzhengheiti|Founder Hei, a Chinese Simplified font|ZmFuZ3poZW5naGVpdGk=|According to the RFC, the padding character = can be omitted, that is, ZmFuZ3poZW5naGVpdGk|
|fangzhengfangsong|Founder FangSong, a Chinese Simplified font|ZmFuZ3poZW5nZmFuZ3Nvbmc=|According to the RFC, the padding character = can be omitted, that is, ZmFuZ3poZW5nZmFuZ3Nvbmc|
|droidsansfallback|DroidSansFallback|ZHJvaWRzYW5zZmFsbGJhY2s=|According to the RFC, the padding character = can be omitted, that is, ZHJvaWRzYW5zZmFsbGJhY2s|

**Text & image watermark**

|Name|Description|Parameter type|
|----|-----------|--------------|
|order| It indicates the order of the text watermark and image watermark of a text & image watermark.

 Value range: \[0, 1\]. 0 \(default\) indicates that the image watermark is before the text watermark; 1 indicates that the text watermark is before the image watermark.

 |Optional|
|align| It indicates the alignment of the text watermark and image watermark of a text & image watermark.

 Value range: \[0, 1, 2\]. 0 \(default\): top alignment; 1: center alignment; 2: bottom alignment

 |Optional|
|interval| It indicates the spacing between the text watermark and image watermark of a text & image watermark.

 Value range: \[0, 1000\]

 |Optional|

**URL-safe Base64 encoding**

Many parameters must be Base64 encoded during image processing. For more information, see [RFC4648](http://www.ietf.org/rfc/rfc4648.txt?file=rfc4648.txt). The URL-safe Base64 encoding is only applicable to some specific watermark parameters \(text content, color, and font of a text watermark, and object of an image watermark\). Do not use it in a signature. The encoding format is:

-   Encode the content to produce a base64 result.
-   Replace the plus sign \(+\) in the result with a minus sign \(-\).
-   Replace the slash sign \(/\) in the result with an underscore \(\_\).
-   Keep all equal signs \(=\) at the end of the result;

An example in Python is shown as follows:

```
import base64
input='wqy-microhei'
print(base64.urlsafe_b64encode(input))
```

## Example {#section_tj2_dbv_vdb .section}

-   The following URL watermarks the file example.jpg with panda.png \(after URL-safe base64 encoded: cGFuZGEucG5n\).

    [http://image-demo.img-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_300,h\_300/auto-orient,1/quality,q\_90/format,jpg/watermark,image\_cGFuZGEucG5n,t\_90,g\_se,x\_10,y\_10](http://image-demo.img-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_300,h_300/auto-orient,1/quality,q_90/format,jpg/watermark,image_cGFuZGEucG5n,t_90,g_se,x_10,y_10)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4787/2654_en-US.jpg)

-   Scale panda.png to 50 in width. Then the watermark file is panda.png?x-oss-process=image/resize,w\_50, and cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLHdfNTA=\) after URL-safe Base64 encoding.

    [http://image-demo.img-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_300,h\_300/auto-orient,1/quality,q\_90/format,jpg/watermark,image\_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLHdfNTA=,t\_90,g\_se,x\_10,y\_10](http://image-demo.img-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_300,h_300/auto-orient,1/quality,q_90/format,jpg/watermark,image_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLHdfNTA=,t_90,g_se,x_10,y_10)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4787/2655_en-US.jpg)


