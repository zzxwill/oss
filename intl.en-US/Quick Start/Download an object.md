# Download an object {#task_wlr_lr4_tdb .task}

After uploading an object to a bucket, you can obtain the URL of the object to download it or share it with other users.

The object has been uploaded to the bucket. For more information, see [Upload an object](reseller.en-US/Quick Start/Upload an object.md#).

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss). 
2.  In the left-side bucket list, click the name of the bucket that you created. 
3.  In the overview page of the bucket, click the Files tab. 
4.  Click the name of the object that you want to download or share, or click **Preview** on the right of the object. In the Preview page, you can see the following options: 
    -   **Download**: Download the object to your local storage device.

        **Note:** Depending on how many objects you require, you can also download objects by using the following methods:

        -   Download multiple objects: On the **Files** tab page, select multiple objects, and then choose **Batch operation** \> **Download**.
        -   Download a single object: On the **Files** tab page, select an object, and then choose **More** \> **Download**.
    -   **Open File URL**: View the object in a browser. The objects that cannot be viewed in a browser \(such as Excel files\) are downloaded when you select this option.
    -   **Copy File URL**: Copy the URL of the object to share it with other users, so that they can use the URL to view or download the object.

        If you want to share the URL of an object within a bucket whose ACL is Private, you must set the **Validity Period** on the Preview page when you want to obtain the URL of an object. The default value of the validity period is 3,600 seconds, and the maximum value is 64,800 seconds.

        **Note:** The validity period of a signed URL is calculated based on NTP. You can share the signed URL of an object to other users so that they can use the URL to access the object within the validity period. If your bucket ACL is Private, a signature is added to the URL of the objects stored in the bucket. For more information, see [Add a signature to a URL](../../../../../reseller.en-US/API Reference/Access control/Add a signature to a URL.md#).

    -   **Copy File Path**: Copy the path of the object. You can use the path when searching for the object or adding watermarks to the object \(if it is a picture\).

