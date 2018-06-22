# CDN-based OSS acceleration {#concept_kfw_2dz_5db .concept}

## Background {#section_bfr_3dz_5db .section}

Structure of traditional products without static-dynamic separation \(however, performance encounters a bottleneck as traffic increases\).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4408/1605_en-US.png)

Product structure implementing static-dynamic separation \(a flexible structure supports massive user traffic\).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4408/1607_en-US.png)

## Scenarios {#section_nyk_tdz_5db .section}

-   Massive access to static files, high server loads, and I/O problems, causing slow user access.
-   Large volumes of static files and insufficient storage space.
-   Massive access to static files distributed across various regions.
-   Fast and concurrent download of mobile update packages in large volumes within a certain time period.

## Structural description {#section_ul4_vdz_5db .section}

As the storage source for massive file volumes, OSS stores static images, video files, downloaded packages, app update packages, and other resources. As OSS is the origin site for CDN, OSS files can be obtained through CDN accelerated delivery by accessing nearby CDN nodes. Structural advantages:

-   Reduces the load on Web servers, and directs the access to all static files to CDN.
-   Provides the lowest storage fees. OSS storage fee is only half that of ECS disks.
-   Provides massive storage capacity, without the need to consider structural upgrade.
-   Minimizes traffic fees. Apart from a small amount of additional origin retrieval traffic, the majority of the traffic is CDN traffic. And it’s cost is lesser than the Internet traffic for direct access from OSS.

## Case study {#section_z5k_wdz_5db .section}

A common website is used as an example. A recently established website www.acar.com, is an automotive news and discussion website which is built on PHP. The main site stores 10 GB of image resources and some JS files. An ECS instance is purchased to store all program codes and MySQL database is installed on the ECS instance. As access traffic continues to grow, many users report that the website access speed experiences a slow down such as loading of pictures and website response consumes time. The website’s technical staff notices that users are uploading an increasing number of images and the total size will soon exceed 1 TB.

The technical staff can use OSS and CDN to optimize the website structure to achieve static-dynamic separation shown in the preceding figure. It enhances user experience, and keep their costs at a manageable level.

The specific solution and procedures are as follows:

1.  Sort out the website program code on the ECS instance by storing dynamic programs and static resources in different directories for better management.
    -   Create a directory named Images for storing the website’s high-definition images.
    -   Create a directory named Javascript for storing all JS scripts.
    -   Create a directory named Attachment for storing all images and attachments uploaded by users.
2.  Create a bucket.

    Select the bucket region based on your ECS region and select the permission option Public Read. You have to make sure that the bucket name corresponds to one of the directories created on the ECS instance, for example, acar-image-bucket. For more information, see [Create a bucket](../../../../intl.en-US/Console User Guide/Manage buckets/Create a bucket.md#).

3.  Enter **image.acar.com**  as the domain for the HD videos and images on your website. For more information, see [Manage a domain name](../../../../intl.en-US/Console User Guide/Manage buckets/Manage a domain.md#).
4.  Upload files to verify the CDN effect.
    1.  Upload all image files in the Images directory created in Step 1 on the ECS instance to **acar-image-bucket**. For more information, see [Upload objects](../../../../intl.en-US/Console User Guide/Manage objects/Upload objects.md#). You can use an OSS client to complete the upload process conveniently.
    2.  Get the CDN address for this file. The address format is `your CDN domain+'/'+'file name'`. For more information, see [Get object URL](../../../../intl.en-US/Console User Guide/Manage objects/Get object URL.md#).
    3.  Upload image files one by one.
5.  Repeat the preceding steps to upload the files in the other two directories, and create the **CDN-based** OSS buckets **acar-js-bucket** and **acar-csimages-bucket** .
6.  In the ECS system, find the access code for the static files and replace the access URL with the CDN domain. Users access static files on your website in OSS+CDN mode without occupying your ECS resources.

    **Note:**  If you want to automatically synchronize user-uploaded files to **acar-csimages-bucket**, see the OSS SDKs and the PutObject section of the API documentation. This provides information on how to perform automatic upload at the code level.


## CDN automatic refresh {#section_p24_b2z_5db .section}

If you use Alibaba Cloud CDN with a bound CDN domain that points back to an OSS source, you can use OSS’s CDN cache automatic refresh function. OSS automatically refreshes CDN when the data is overwritten \(for example, when an existing file is overwritten or deleted\). A origin retrieval operation is performed to obtain the overwritten file from OSS, so you do not need to explicitly call the CDN refresh interface. The URL refresh rules are as follow: `+ / + Object`

For example, if the uploaded file `test.jpg` is overwritten in the bucket bound to the CDN domain `image.acar.com`, OSS refreshes the `image.acar.com/test.jpg` URL. The time required by the refreshed URL to take effect is determined by CDN’s guaranteed refresh time, which is typically less than 10 minutes.

To activate CDN-based OSS acceleration, on the Bucket Properties \> Domain Management page, enable the ** **function.

