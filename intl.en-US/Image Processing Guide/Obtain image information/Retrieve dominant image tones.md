# Retrieve dominant image tones {#concept_hsb_wcv_vdb .concept}

You can retrieve the average tones of images.

## Request operation {#section_psd_xcv_vdb .section}

Operation name: `average-hue`

## Return format {#section_xdx_ycv_vdb .section}

0xRRGGBB \(RR, GG, and BB are hexadecimal values respectively indicating red, green, and blue\)

## Example {#section_a4d_1dv_vdb .section}

Access the following URL through a browser:

[http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/average-hue](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/average-hue)

The following result is returned:

```
{"RGB": "0x5c783b"}
```

Source image:

[http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4789/2667_en-US.jpg)

0x5c783b corresponds to the color RGB \(92,120,59\).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4789/2668_en-US.png)

