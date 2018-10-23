# Host a static website {#concept_ars_bhz_5db .concept}

You can set your bucket to host a static website and access this static website through the bucket domain name.

-   If the default webpage is blank, static website hosting is disabled.
-   If static website hosting is enabled, we recommend that you use CNAME to bind your domain name.
-   If you directly access the static website root domain or any URL ending with “/“ under this domain, the default homepage is returned.

**Note:** When you use an OSS endpoint in Mainland China regions or the Hongkong region to access a web file through the Internet , the Content-Disposition: 'attachment=filename;' is automatically added to the Response Header, and the web file is downloaded as an attachment. If you access OSS with a user domain, the Content-Disposition: 'attachment=filename;' will not be added to the Response Header. For more information about using the user domain to access OSS, see [Bind a custom domain name](../../../../reseller.en-US/Developer Guide/Access and control/Bind a custom domain name.md#).

For more information, see [Static Website Hosting](../../../../reseller.en-US/Developer Guide/Static website hosting/Configure static website hosting.md#).

## Procedure {#section_vlx_ghz_5db .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left bucket lists, click one target bucket name to open the bucket overview page.
3.  In the **Basic Settings** tab, find the **Static Page** area.
4.  Click **Settings** to set the following parameters:
    -   Default Homepage: that is the index page \(equivalent to the website’s index.html\), only HTML files that have been stored in the bucket can be used. If this field is left empty, the default home page settings are not enabled.
    -   Default 404 Page: The default 404 page returned when an incorrect path is accessed, only html, jpg, png, bmp, and webp files that have been stored in the bucket can be used. If this field is left empty, the default 404 page is disabled.
5.  Click **Save**.

