# Indexed cut {#concept_ecq_cqs_vdb .concept}

This feature allows you to define an x and y-axes coordinate system for an image, and then fetch a specific partition of the image by specifying the length and index.

## Parameters {#section_fy1_2qs_vdb .section}

This table provides the description and values for the parameters for the `indexcrop` operation.

|Parameter|Description|Value|
|---------|-----------|-----|
|X|Length of each image partition during horizontal cutting. Either the x or y parameter must be used.|\[1, image width\]|
|Y|Length of each image partition during vertical cutting. Either the x or y parameter must be used.|\[1, image height\]|
|i|Image partition selected after cutting. \(The value 0 indicates the first partition.\)|\[0, maximum partition quantity\). If the maximum partition quantity is exceeded, the system returns the source image.|

## Caveats {#section_uzq_nqs_vdb .section}

If the specified index exceeds the cut range, the system returns the source image.

## Example {#section_b5g_pqs_vdb .section}

Divide an image equally by 100 on the X-axis, and fetch the first partition.

-   Divide an image equally by 100 on the X-axis, and fetch the first partition.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/indexcrop,x\_100,i\_0](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/indexcrop,x_100,i_0)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4773/2498_en-US.jpg)

-   Divide an image equally by 100 on the X-axis, fetch the 100th partition, and check whether the source image is returned.

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/indexcrop,x\_100,i\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/indexcrop,x_100,i_100)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4773/2500_en-US.jpg)


