# Download an object {#concept_k4p_dvl_vdb .concept}

After uploading an object to a bucket, you can share or download the object.

## Procedure {#section_g3q_2vl_vdb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  In the left-side bucket list, click the name of the target bucket.
3.  In the overview page of the bucket, click the **Files** tab.
4.  Click the name of the target file. In the Preview page of the file, you can perform the following operations:

    -   **Download**: You can download the file to your local storage.

        You can also download files in the following methods:

        -   Download one or more files: On the **Files** page, select one or more files, and then select **Batch operation** \> **Download**.
        -   Download a single file: On the **Files** page, select a file, and then select **More** \> **Download**.
    -   **Open File URL**: You can directly open the file in a browser. A file that cannot be opened directly, such as Excel files, is downloaded when the URL is opened.
    -   **Copy File URL**: You can obtain the URL of a file and share it with others, allowing them to browse and download the file.

        You can also obtain the URL of a file in the following methods:

        -   Obtain the URL of one or more files: On the **Files** page, select one or more files, and then select **Batch operation** \> **Export URL List**.
        -   Obtain the URL of a single file: On the **Files** page, select **More** \> **Copy File URL**.
    -   **Copy File Path**: You can search a file or add watermarks to an image file.
    **Warning:** If the bucket is configured with Referer Whitelist and Empty Referer is not allowed, then the URL cannot be opened directly in a browser.

5.  If your bucket ACL is set to **Private**, you must set the **Validity Period** in the **Signature** field when obtaining the URL of a file. The validity period is 3,600 seconds by default and cannot exceed 64,800 seconds.

    **Note:** 

    -   The validity period of a signed URL is calculated based on NTP. You can share a URL to others, allowing them to access the file within the validity period. If your bucket ACL is set to Private, a signature will be added to the URL. For more information, see [Add a signature to a URL](../../../../intl.en-US/API Reference/Access control/Add a signature to a URL.md#).
    -   You can change the ACL of a bucket or a file anytime. For more information, see [Change bucket ACL](intl.en-US/Console User Guide/Manage buckets/Change bucket ACL.md#) and [Change object ACL](intl.en-US/Console User Guide/Manage objects/Change object ACL.md#).

