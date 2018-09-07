# Download an object {#task_wlr_lr4_tdb .task}

After uploading an object to a bucket, you can download the object or share it with others.

The object is uploaded to a bucket. For more information, see [Upload an object](intl.en-US/Quick Start/Upload an object.md#).

1.  Log on to the [OSS console](https://oss.console.aliyun.com/). 
2.  In the bucket name list, click the name of the bucket that you created. 
3.  Click the Files tab. 
4.  Click the name of the file that you uploaded, or click **Configure** to open the Preview page. You can see the following options: 

    -   **Download**: used to download a file to your local PC.
    -   **Open File URL**: used to open the file in a browser. Files that cannot be opened directly, such as Excel files, are downloaded directly when the URL is opened.
    -   **Copy File URL**: used to give the URL to anyone who needs to open or download the file.
    -   **Copy File Path**: used to search a file or place watermarks on an image file.
    **Note:** You can also download files by the following methods:

    -   Locate the target file, and then select **More** \> **Download**.
    -   Select one or more files, and then select **Batch operation** \> **Download**.
5.  If your bucket ACL is Private, you must set **Validity Period** when getting a file URL. 

    **Note:** The validity period of a signed URL is calculated based on NTP. You can give the URL to anyone who can then use it to access the file within the validity period. If the bucket ACL is set as Private, a signature will be added to the URL. For more information, see [Add a signature to a URL](https://www.alibabacloud.com/help/doc-detail/31952.htm).


