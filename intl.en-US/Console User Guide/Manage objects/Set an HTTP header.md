# Set an HTTP header {#concept_pk1_sxl_vdb .concept}

HTTP header is used to define the policy of HTTP requests, such as cache policy or download policy. You can set an HTTP header for up to 1,000 files using the batch process in the OSS console.

**Note:** If you want to set an HTTP header for more than 1,000 files, see the Java SDK document [Manage Object Meta](https://www.alibabacloud.com/help/doc-detail/84840.htm).

## Procedure {#section_i1g_cyl_vdb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  In the left-side bucket list, click the name of the target bucket to open the overview page of the bucket.
3.  Click **Files**.
4.  Select one or multiple files, and then select **Batch operation** \> **Set HTTP Header**.

    You can also set an HTTP header for a single file as follows: Click the target file name, and then click **Set HTTP Header** in the Preview page.

    -   To set the HTTP header for one or multiple files, select one or multiple files, and then select **Batch operation** \> **Set HTTP Header**.
    -   To set the HTTP header for a single file, click **Configure** for the file. On the Preview page, click **Set HTTP Header**.
    -   To set the HTTP header for a single file, you can also select **More** \> **Set HTTP Header**.
5.  Set the related parameters. You can also add user-defined metadata.

    **Note:** For more information about each parameter, see [Definitions of common HTTP headers](../../../../intl.en-US/API Reference/Definitions of common HTTP headers.md#).

6.  Click **OK**.

