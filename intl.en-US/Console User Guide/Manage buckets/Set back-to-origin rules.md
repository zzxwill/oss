# Set back-to-origin rules {#concept_ukn_3tf_vdb .concept}

This topic describes how to set back-to-origin rules.

If you access data that does not exist on OSS, a 404 Not Found error is return. However, if you set back-to-origin rules and set the correct URL for the data, you can obtain the data correctly through the back-to-origin rules. Two kinds of back-to-origin rules can be set: mirroring rules and redirection rules, which can meet the requirements of frequently accessed \(hot\) data migration and specific request redirection requirements. You can set a maximum of 5 back-to-origin rules, which are compared with access requests in a sequence that they are set. For more information, see [Manage back-to-origin settings](../../../../../reseller.en-US/Developer Guide/Manage files/Manage back-to-origin settings.md#).

 

 

## Procedure {#section_edr_sfg_vdb .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the bucket that you want to set back-to-origin rules for.
3.  In the overview page of the bucket, click the **Basic Settings** tab, locate the **Back-to-Origin** area, and click **Configure**.
4.  Click **Create Rule**.
5.  In the **Create Rule** dialog box, configure parameters as follows to create a back-to-origin rule.

    |Parameter|Required|Description|
    |:--------|:-------|:----------|
    |**Mode**|Yes|Select **Mirroring** or **Redirection**.    -   In the **Mirroring**mode, if the object requested by the user does not exist in OSS, OSS obtains the object from the origin site and returns the object to the user.
    -   In the **Redirection** mode, requests that meet the response requirement are redirected to specified URLs using the HTTP redirection method. Browsers or clients obtain the request data from the origin site through the URLs.
|
    |**Prerequisite**|Yes|Set the conditions that trigger the back-to-origin rule. A rule is triggered only when all the conditions are met.    -   **HTTP Status Code**: The back-to-origin rule is triggered when the specified HTTP status code is returned. The default HTTP status code is 404, that is, the rule is triggered when the requested object does not exist in OSS and a 404 HTTP status code is returned.

This condition is selected by default and cannot be configured when you select the **Mirroring** mode, and is optional when you select the **Redirction** mode.

    -   **File Name Prefix**: The back-to-origin rule is triggered when objects with the specified prefix are accessed. For example, if you set the prefix to abc/, the rule is triggered when the `http://bucketname.oss-endpoint.com/abc/image.jpg` object is accessed.

**Note:** This condition is optional when you create a single back-to-origin rule. When creating multiple back-to-origin rules, you must set different prefixes for different rules.

|
    |**Origin URL**|Yes|Sets the origin URL.Directories with multiple levels must be separated by slashes \(/\). If you select the Redirection mode, a directory must end with a slash \(/\), and asterisks \(\*\) are not supported.

    -   If you select the **Mirroring** mode, configure the origin URL as follows:
        -   First column:Select **http** or **https** based on the type of the origin site.
        -   Second column: Input the domain name or IP address of the origin site.

**Note:** Input the IP address only when the origin site can be directly accessed through the IP address.

        -   Third column: Input the directory where the requested object is stored. For example, abc/123.
    -   If you select the **Redirection** mode, you must set the processing rule for the origin URL. The configurations of the first and second column are similar to those when you select the **Mirroring** mode.
        -   **Add Prefix or Suffix**: Adds a prefix or suffix to the redirected URL, in which the prefix is configured in the third column and the suffix is configured in the fourth column.

You can select this option to configure correct information for the redirected URL when the URL of the requested data does not have a prefix or suffix. For example, if you set the prefix to `123/` and the suffix to `.jpg`, access to `http://bucketname.oss-endpoint.com/image` is automatically redirected to `http://xx.com/123/image.jpg`.

        -   **Redirect to Fixed URL**: Access to the requested object is redirected to a specified object. You must input the information about the object that is redirected to, for example, abc/myphoto.jpg.

**Note:** You can also set the URL to a website. For example, if you set the URL to `https://www.aliyun.com/index.html`, access to the bucket is redirected to the Alibaba Cloud home page.

        -   **Replace File Name Prefix**: If you set the **File Name Prefix** for **Prerequisite** and select this option, the prefix in the name of the object that the access is redirected to is replaced. If you do not set the **File Name Prefix** for **Prerequisite** and select this option, the specified prefix is added to the name of the object that is redirected to.
|
    |**Other parameters**|No \(only applies for the Mirroring mode\)|**Transfer queryString**: The queryString included in the request to OSS is transferred to the origin site.|
    |**3xx Response**|No \(only applies for the Mirroring mode\)|**Follow the source station to redirect request**: The requested data is obtained through the 3xx redirection request sent to the origin site and stored in OSS by default. If you do not select this option, OSS passes through the 3xx response and does not obtain the data.|
    |**Set transmission rule of HTTP header**|No \(only applies for the Mirroring mode\)|You can set transmission rule for HTTP headers to customize transmission actions, such as passthrough, filtering, or modifying. For more information, see the following [examples of setting transmission rules for HTTP headers](#).|
    |**Redirection Code**|Yes \(only applies for the Redirection mode\)|You can select the redirection code.**Source from Alibaba Cloud CDN**: Select this option if the origin site is from Alibaba Cloud CDN.

|

    Examples of setting transmission rules for HTTP headers are listed as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4750/15531342509983_en-US.png)

    If the HTTP header in a request that sent to OSS is as follows:

    ```
    GET /object
    host : bucket.oss-cn-hangzhou.aliyuncs.com
    aaa-header : aaa
    bbb-header : bbb
    ccc-header : 111
    ```

    After the back-to-origin is triggered, the request that OSS sends to the origin site is as follows:

    ```
    GET /object
    host : source.com
    aaa-header : aaa
    ccc-header : ccc
    
    ```

    **Note:** Transmission rules do not support the following HTTP headers:

    -   Headers with the following prefixes:
        -   x-oss-
        -   oss-
        -   x-drs-
    -   All standard HTTP headers, such as:
        -   content-length
        -   authorization2
        -   authorization
        -   range
        -   date
6.  Click **OK**.

**Note:** After a back-to-origin rule is saved, you can view the configured rule in the rule list and perform Edit or Clear operations.

