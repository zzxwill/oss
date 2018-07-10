# Share an object {#task_wlr_lr4_tdb .task}

After an object is uploaded to a bucket, the object can be shared and downloaded by others.

1.   Log on to the [OSS console](https://oss.console.aliyun.com/). 
2.   In the bucket name list, click the name of the bucket that you created. 
3.   Click the Files tab. 
4.   Click the name of the file that you uploaded to open the Preview page. You can see the following options under the URL field.
    -   **Open File URL**: used to open the file in the browser.
    -   **Copy File URL**: used to copy the file URL and share it with others.
    -   **Copy File Path**: used to search a file or watermark an image file.
5.   Click **Copy File URL** and give it to any user who needs to browse or download the file. For the files that do not support browsing directly, such as an Excel file, open the URL and file is downloaded directly. 

    If your bucket ACL is Private, you must set the Validity when getting a file URL.

    **Note:** The validity period of a signed URL is calculated based on NTP. You can give this URL to any visitor who can then use it to access the file within the validity period. If the bucket ACL has a private permission, a signature will be added to the URL. For more information, see [Add a signature to a URL](../../../../intl.en-US/API Reference/Access control/Add a signature to a URL.md#).


