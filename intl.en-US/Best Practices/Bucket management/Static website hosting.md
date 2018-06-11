# Static website hosting {#concept_trc_jn2_vdb .concept}

This document describes the process and procedure about how to build a simple static website based on OSS right from the beginning and also includes FAQs as well. The following are the key steps:

1.  Apply for a domain name.
2.  Activate OSS and create a bucket.
3.  Activate Static Website Hosting on OSS.
4.  Access OSS with custom domain names.

Static website hosting overview

## FAQs about static website hosting {#section_emx_kn2_vdb .section}

You can build a simple static website page based on OSS. Once you activate this function, OSS provides a default homepage and a default 404 page. For more information, see [Static Website Hosting](../intl.en-US/Developer Guide/Static Web Hosting/Configure static website hosting.md#) in the developer guide.

## Procedure {#section_yg1_mn2_vdb .section}

1.  Apply for a domain name
2.  Activate OSS and create a bucket
    1.  Log on to the OSS console and create a bucket named “imgleo23” in Shanghai with the endpoint `oss-cn-shanghai.aliyuncs.com`. For detailed operation, see [Create a bucket](../intl.en-US/Console User Guide/Manage buckets/Create a bucket.md#).
    2.  Set the bucket permission to public-read. For detailed operation, see [Set bucket ACL](../intl.en-US/Console User Guide/Manage buckets/Change bucket ACL.md#).
    3.  Upload the content of index.htm and error.htm.  For detailed operation, see [Upload files](../intl.en-US/Console User Guide/Manage objects/Upload objects.md#).
        -   Body of  index.html:

            ```
            <html>
              <head>
                  <title>Hello OSS! </title>
                  <meta charset="utf-8">
              </head>
              <body>
                  <p>Welcome to OSS Static Website Hosting.</p>
                  <p>This is the homepage.</p>
              </body>
            </html>
            ```

        -   Body of error.html:

            ```
            <html>
              <head>
                  <title>Hello OSS! </title>
                  <meta charset="utf-8">
              </head>
              <body>
                  <p>This is an error homepage for OSS Static Website Hosting.</p>
              </body>
            </html>
            ```

        -   aliyun-logo.png is a picture.
3.  Activate static website hosting on OSS

    As shown in the following figure, once you log on to the OSS console, set Default Homepage to index.html and Default 404 Page to error.html. For more information, see [Set static website hosting](../intl.en-US/Console User Guide/Manage buckets/Host a static website.md#).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4412/1739_en-US.png)

    To test the Static Website Hosting function, enter the URL as shown in the following figure:

    -   Display the default homepage: 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4412/1742_en-US.png)

        When a similar URL is entered, the body of index.html specified upon activating the function is displayed.

    -   Display normal files 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4412/1743_en-US.png)

        When a matched file for the entered URL is found, data is read successfully.

4.  Access OSS with custom domain names

    For more information about how to access OSS with custom domain names, see [Access OSS with custom domain names](../intl.en-US/Developer Guide/Access and control/Bind a custom domain name.md#).

    -   Display the default homepage 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4412/1746_en-US.png)

    -   Display the default 404 page: 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4412/1748_en-US.png)

    -   Display normal files 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4412/1749_en-US.png)


## FAQ {#section_iqr_5df_vdb .section}

-   What are the benefits of OSS Static Website Hosting?

    An ECS instance is saved in case any user needs a relatively small amount of traffic. In the case of larger traffic volumes, CDN can be used.

-   How is OSS priced? How does OSS work with CDN?

    For pricing, see the OSS and CDN prices on Alibaba Cloud website.   For cases on combination of OSS and CDN, see[CDN-based OSS acceleration practices](intl.en-US/Best Practices/Bucket management/CDN-based OSS acceleration.md#).

-   Do the default homepage and default 404 page both need to be set?

    The default homepage needs to be set, whereas the default 404 page does not need to be set.

-   Why does the browser return a 403 error after a URL is entered?

    The reason may be that the bucket permission is not public-read, or your Static Website Hosting function is suspended due to overdue payment.


