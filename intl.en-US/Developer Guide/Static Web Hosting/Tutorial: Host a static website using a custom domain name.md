# Tutorial: Host a static website using a custom domain name {#concept_snk_r3c_5db .concept}

Suppose that you want to host a static website on Alibaba Cloud Object Storage Service \(OSS\). You have registered a domain \(for example, examplewebsite.com\), and you want the requests for `http://examplewebsite.com` and `http://www.examplewebsite.com` to be serviced from your OSS content. Whether you have an existing static website that you want to host on OSS, or you are starting from scratch, you can use this example and learn how to host websites on Alibaba Cloud OSS.

## Prerequisites {#section_kz4_1jc_5db .section}

This tutorial covers the following services:

-   Domain name registration

    If you do not have a registered domain name, such as `examplewebsite.com`, select a registrar to register one. Alibaba Cloud also provides domain name registration service. For more information, see [Alibaba Cloud Domain service](https://www.alibabacloud.com/help/doc-detail/61257.htm).

-   Alibaba Cloud OSS

    You use Alibaba Cloud OSS to create buckets, upload a sample website page, configure permissions to let others see the content, and then configure the buckets for website hosting. In this example, because you want to allow requests for  `http://examplewebsite.com` and `http://www.examplewebsite.com`, you create two buckets.  You host content in only one bucket, and configure the other bucket to redirect requests to the bucket that hosts the content.

-   Alibaba Cloud DNS

    As your Domain Name System \(DNS\) provider, you configure Alibaba Cloud DNS. In this example, you add your domain name to Alibaba Cloud DNS and define a CNAME record so that you can use your domain name instead of the OSS assigned access domain name to access your OSS buckets.

    -   In this example, we use Alibaba Cloud DNS. We recommend that you use Alibaba Cloud DNS. However, you can use various registrars to define a CNAME record that points to an OSS bucket.


**Note:** 

This tutorial uses `examplewebsite.com` as a domain name. Replace this domain name with the one that you have registered.

## Step 1: Register a domain {#section_jww_3jc_5db .section}

If you already have a registered domain, you can skip this step. If you have never hosted a website, your first step is to register a domain, such as `examplewebsite.com`. You can use Alibaba Cloud Domain service to register a domain.

For more information, see [Buy a domain name](https://www.alibabacloud.com/help/doc-detail/54068.htm) in the Alibaba Cloud Domain Quick Start. 

## Step 2: Create and configure buckets and upload data {#section_e5f_kjc_5db .section}

You create two buckets to support requests from both the root domain such as `examplewebsite.com` and subdomain such as `http://www.examplewebsite.com`. One bucket is used to store the content, and the other bucket is used to redirect requests to the bucket that stores the content.

Step 2.1: Create two buckets

In this step, you log on to Alibaba Cloud OSS console with your Alibaba Cloud account credentials and create the following two buckets:

-   **originbucket**:  to store the content
-   **redirectbucket**: to redirect requests to **originbucket**

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  Create two buckets, for example, **originbucket** and **redirectbucket**, one to store the content, and the other to redirect requests to the bucket that stores the content. Set the access control list \(ACL\) of the two buckets to Public Read so that everyone can see the content of the buckets.

    For detailed procedures, see [Create a bucket](../intl.en-US/Console User Guide/Manage buckets/Create a bucket.md#).

3.  Make yourself a note of the access domain name of the **originbucket** and **redirectbucket**. You will use them in later steps. You can find the access domain name of a bucket on the Overview tab page of the bucket, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4388/1078_en-US.png)

4.  Upload your website data to **originbucket**.

    You will host your content out of the root domain bucket **originbucket**, and you will redirect requests for the subdomain bucket **redirectbucket** to the root domain bucket **originbucket**. You can store content in either bucket.

    For this example, you host content in the **originbucket** bucket. The content can be any type of files, such as text files, photos, and videos. If you have not yet created a website, then you only need two files for this example. One file is used as the homepage of the website, and the other file is used as the error page of the website.

    For example, you can create one file named index.html using the following HTML and upload it to the bucket. In a later step, you use this file name as the default homepage for your website.

    ```
    <html>
       <head>
           <title>Hello OSS! </title>
           <meta charset="utf-8">
       </head>
       <body>
           <p>Now host on Alibaba Cloud OSS</p>
           <p>This is the index page</p>
       </body>
     </html>
    ```

    You create another file named error.html using the following HTML and upload it to the bucket. This file is used as the 404 error page of a website. In a later step, you use this file name as the default 404 page for your website.

    ```
    <html>
    <head>
       <title>Hello OSS! </title>
       <meta charset="utf-8">
    </head>
    <body>
       <p>This is the 404 error page</p>
    </body>
    </html>
    ```


Step 2.2: Configure buckets for website hosting

When you configure a bucket for website hosting, you can access the website using the OSS assigned access domain name.

In this step, you configure originbucket as a website.

1.  Log in to the [OSS console](https://oss.console.aliyun.com/).
2.  From the bucket name list, select originbucket.
3.  Click the Basic Settings tab and find the Static Page area.
4.  Click **Edit**, and then enter the following information:
    -   **Default Homepage**: The index page \(equivalent to index.html of the website\). Only HTML files that have been stored in the bucket can be used.  For this example, enter index.html.

    -   **Default 404 Page**: The default 404 page returned when an incorrect path is accessed. Only HTML and image files that have been stored in the bucket can be used. If this field is left empty, the default 404 page is disabled. For this example, enter error.html.

5.  Click **Save**.

Step 2.3: Configure the index page for redirect

Configuration Now that you have configured the default homepage and error page of the **originbucket**, you also need to configure the default homepage of **redirectbucket**. To configure

the index page for redirect, follow these steps:

1.  Log in to the [OSS console](https://oss.console.aliyun.com/).
2.  From the bucket name list, select redirectbucket.
3.  Click the Basic Settings tab and find the Static Page area.
4.  Click **Edit**, and then enter index.html in the Default Homepage text box.
5.  Click **Save**.

## Step 3: Bind your domain name to your OSS buckets {#section_kgd_tnc_5db .section}

Now that you have your root domain `examplewebsite.com` and your OSS bucket **originbucket**, bind your domain to your OSS buckets so that you can access the OSS buckets using your own domain name instead of the domain name assigned by OSS.

In this example, before you bind your domain `examplewebsite.com` to your OSS bucket **originbucket**, you have to use the OSS assigned domain name originbucket.oss-cn-beijing.aliyuncs.com to access your bucket originbucket. After you bind your domain `examplewebsite.com`, you can use this `examplewebsite.com` to access your OSS bucket.

Similarly, you also need to bind your subdomain `www.examplewebsite.com` to your OSS bucket **redirectbucket**, so that you can use `www.examplewebsite.com` instead of the OSS assigned domain name originbucket.oss-cn-beijing.aliyuncs.com to access your OSS bucket.

To bind your root domain `examplewebsite.com` to your OSS bucket **originbucket**, follow these steps:

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  From the bucket name list, select originbucket.
3.  Click the Domain Names tab.
4.  Click **Bind User Domain** to open the Bind User Domain dialog box.
5.  In the User Domain text box, enter the root domain examplewebsite.com.
6.  Define a CNAME record to originbucket.
    -   If your domain name has been resolved under your Alibaba Cloud account, you can open the **Add CNAME Record Automatically** switch. Then click **Submit**.

    -   If your domain name has not been resolved under your Alibaba Cloud primary account, the **Add CNAME Record Automatically** switch is disabled. Follow these steps to add a CNAME record manually, and then click **Submit**.

        1.  Add your domain name in Alibaba Cloud DNS.
            -   If your domain name is registered with Alibaba Cloud, it is automatically added to the Alibaba Cloud DNS list. You can skip this step.
        2.  In the Alibaba Cloud DNS console, find your domain name.
        3.  Click the domain name or click the \*\*Configure\*\* link.
        4.  Click **Add Record**.
        5.  In the Add Record dialog box, select CNAME from the Type drop-down box, and enter the OSS domain name of the bucket in the Value text box.  In this example, enter **originbucket.oss-cn-beijing.aliyuncs.com**.
        6.  Click **Confirm**.
7.  Follow the preceding steps to bind your sub domain `www.examplewebsite.com` to your OSS bucket **redirectbucket**.

## Step 4: Configure your website redirect {#section_cgg_dvc_5db .section}

Now that you have configured your bucket for website hosting and bound your custom domain to your OSS bucket, configure the **redirectbucket** to  redirect all requests for `http://www.examplewebsite.com` to `http://examplewebsite.com`.

To configure your website redirect, follow these steps:

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  From the bucket name list, select redirectbucket.
3.  Click the Basic Settings tab and find the Back-to-Origin area.
4.  Click **Edit**, and then click **Create Rule**.
5.  Create the 404 redirect rule as follows:
    1.  In the Back-to-Origin Type area, select Redirect.
    2.  In the Back-to-Origin When area, set HTTP Status Code to **404**.
    3.  In the Back-to-Origin URL area, select Add a prefix or suffix, enter your domain name of the **originbucket**.  In this example, enter **examplewebsite.com**.
    4.  Click **OK**.

## Step 5: \(Optional\) Speed up your website with Alibaba Cloud CDN {#section_os3_nwc_5db .section}

You can use Alibaba Cloud Content Delivery Network \(CDN\) to improve the performance of your website. CDN makes your website files \(such as HTML, images, and video\) available from data centers around the world. These are called edge nodes. When a visitor requests a file from your website, Alibaba Cloud CDN automatically redirects the request to a copy of the file at the nearest edge node. This results in faster download times than if the visitor had requested the content from a data center that is located farther away.

Alibaba Cloud CDN caches content at edge nodes for a period of time that you specify. If a visitor requests content that has been cached for longer than the expiration date, CDN checks the origin server to see if a later version of the content is available. If a later version is available, CDN copies the new version to the edge node. Changes that you make to the original content are replicated to edge nodes as visitors request the content. However, before the expiration date, the content is still in the earlier version. We recommend that you open the **Auto Refresh CDN Cache** switch, so that changes you make to the original content are automatically refreshed in CDN cache in real time.

To speed up **originbucket** with CDN, follow these steps:

1.  Add a CDN domain in the CDN console. For detailed procedures, see Step 2. Add a CDN domain in [CDN quick start](https://www.alibabacloud.com/help/doc-detail/27112.htm).
2.  Define a CNAME record in the Alibaba Cloud DNS console. For detailed procedures, see [Configure Alibaba Cloud DNS](https://www.alibabacloud.com/help/doc-detail/27144.htm).
3.  Open the **Auto Refresh CDN Cache** switch in the OSS console.
    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
    2.  From the bucket name list, select originbucket.
    3.  Click the Domain Names tab.
    4.  Open the **Auto Refresh CDN Cache** switch.
4.  Follow the preceding steps to speed up redirectbucket with CDN.

## Step 6: Test the website {#section_rdq_f2d_5db .section}

To verify that the website is working correctly, in your browser, try the following URLs:

|URL|Result|
|:--|:-----|
|`http://examplewebsite.com`|Displays the index document in the **originbucket**.|
|The URL of a file that does not exist in the **originbucket**, for example, http://examplewebsite.com/abc ``|Displays the error document in the **originbucket**.|
|`http://www.examplewebsite.com`|Redirects your request to `http://examplewebsite.com`.|
|``|Redirects your request to `http://examplewebsite.com/abc`.|

**Note:** In some cases, you may need to clear the cache of your web browser to see the expected behavior.

## Cleanup {#section_cvk_jfd_5db .section}

If you created your static website as a learning exercise only, remember to delete the Alibaba Cloud resources that you allocated to avoid unnecessary fees charged to your account.  After you delete your Alibaba Cloud resources, your website is no longer available.

To clean up, follow these steps:

1.  Disable and then remove the domain name in the Alibaba Cloud CDN console.
2.  Delete the CNAME records in the Alibaba Cloud DNS console.
3.  Delete the OSS files and buckets in the Alibaba Cloud OSS console.

