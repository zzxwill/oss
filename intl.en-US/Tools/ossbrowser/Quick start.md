# Quick start {#concept_xmg_h33_wdb .concept}

Ossbrowser is a graphical management tool developed by Alibaba Cloud. It provides features similar to those of Windows Explorer. Using ossbrowser, you can view, upload, download, and manage objects with ease.

**Note:** 

-   You can only move or copy objects smaller than 5 GB by using ossbrowser. For objects larger than 5 GB, we recommend you use [ossutil](reseller.en-US/Tools/ossutil/Quick Start.md#).
-   Ossbrowser supports Linux, Mac, and Windows \(Windows 7 and later\). We recommend you do not use ossbrowser in Windows XP and Windows Server.

## Installation {#section_mq4_l33_wdb .section}

1.  Download and install ossbrowser

    |Supported operating system|Download URL|
    |:-------------------------|:-----------|
    |Windows x32|[Windows x32](http://gosspublic.alicdn.com/oss-browser/1.9.1/oss-browser-win32-ia32.zip)|
    |Windows x64|[Windows x64](http://gosspublic.alicdn.com/oss-browser/1.9.1/oss-browser-win32-x64.zip)|
    |MAC|[MAC](http://gosspublic.alicdn.com/oss-browser/1.9.1/oss-browser-darwin-x64.zip)|
    |Linux x64|[Linux x64](http://gosspublic.alicdn.com/oss-browser/1.9.1/oss-browser-linux-x64.zip)|

    **Note:** For more download URLs, see [GitHub](https://github.com/aliyun/oss-browser/blob/master/all-releases.md).

2.  Start ossbrowser.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/155558042040359_en-US.png)

    Set the following parameters to log on to ossbrowser:

    -   **Endpoint**: Select the region \(endpoint\) that you want to log on.
        -   Default: Log on to ossbrowser with the default endpoint.
        -   Customize: Enter the endpoint you want to use to log on to ossbrowser. You can enter a URL starting with "http" or "https" to log on to ossbrowser through the HTTP or HTTPS method, for example, https://oss-cn-beijing.aliyuncs.com。For more information about the regions and endpoints, see [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).
        -   cname: You can log on to ossbrowser with a custom domain name \(CNAME\) attached to your OSS resources. For more information about attaching a CNAME, see [Attach a custom domain name](../../../../reseller.en-US/Console User Guide/Manage buckets/Manage a domain/Attach a custom domain name.md#).
    -   AccessKeyId/AccessKeySecret：Enter the Accesskey \(AK\) of your account. To ensure data security, we recommend that you use the AK of a RAM user to log on to ossbrowser. For more information about AK, see [Create an AccessKey](../../../../reseller.en-US/General Reference/Create an AccessKey.md#).
    -   **Preset OSS Path**:
        -   Administrator RAM users with administration permissions on all buckets: No configuration is required.
        -   Operator RAM users: Configurations are required. Enter the path of the OSS bucket or sub-directory that you want to access \(the RAM user must have permission to access the OSS bucket or sub-directory\). The path format is as follows: **oss:// bucket name/sub-directory name/**.
    -   **Region**: Select the region where the OSS resources belong to.
    -   **Remember**: Select to save the AK. When you log on to ossbrowser later, you can simply click **AK Histories** and select the saved AK instead of entering the AK repeatedly. Do not select this option if you use a shared computer.

## Usage {#section_opg_4mh_1hb .section}

Ossbrowser supports simple management operations on OSS resources.

-   Manage a bucket
    -   Create a bucket.
        1.  On the main interface of ossbrowser, click **Create Bucket**.
        2.  Set the following information about the bucket:
            -   **Name**: The name of a bucket can be 63 characters in maximum and must be unique.
            -   **Region**: Select the region where the bucket belongs to.
            -   **ACL**: Select the ACL for the bucket. For more information about ACL, see [ACL](../../../../reseller.en-US/Developer Guide/Access and control/ACL.md#).
            -   **Type**: Select the default storage class of the bucket. For more information about storage class, see [Introduction to storage classes](../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#).
        3.  Click **OK**.
    -   Delete a bucket.

        Select the bucket that you want to delete, and then click **More** \> **Remove**.

        **Note:** A bucket cannot be deleted when objects or parts are stored in it.

-   Manage objects/directories
    -   Create a directory.
        1.  On the main interface of ossbrowser, click the bucket in which you want to create a folder.
        2.  Click **Directory**.
        3.  Enter the name of the directory and click **OK**.

            **Note:** 

            -   Emoticons are not allowed in a directory name. Use compliant UTF-8 characters in directory names.
            -   You can create only a single-level directory at a time. For example, you can create a single-level directory abc but not a multi-level directory abc/123.
            -   A sub-directory named .. is not allowed.
            -   The length of a directory name must be in a range of 1 to 254 characters.
    -   Upload files/directories.

        In the specified bucket or directory, click **Files**/**Folder**, and then select the files or folders that you want to upload.

        **Note:** You can upload multiple files or folders at the same time.

    -   Download objects/directories

        In the specified bucket or directory, select the objects or directories that you want to download, and then click **Download**.

        **Note:** You can download multiple objects or folders at the same time.

    -   Copy objects/directories.
        1.  In the specified bucket or directory, select the objects or directories that you want to copy, and then click **Copy**.
        2.  Enter the bucket or directory where you want to copy the data to, and then click **Paste**.

            **Note:** If the source address and target address of the copied object are the same, the original object is overwritten. If the storage class of the overwritten object is IA or Archive and the storage period of the object does not reach the required value, fees incur for the advanced deletion. For more information, see [Billing items](../../../../reseller.en-US/Pricing/Billing items.md#table_v24_5ft_lgb).

    -   Move objects/directories.
        1.  In the specified bucket or directory, select the objects or directories that you want to move, and then click **More** \> **Move**.
        2.  Enter the bucket or directory where you want to move the data to, click **Paste**.

            **Note:** When you move an object or a directory, the object or directory is copied from the source address to the target address, and the object or directory in the source address is deleted. If you move an object of the IA or Archive storage class and the storage period of the object does not reach the required value, fees incur for the advanced deletion.

    -   Rename objects/directories

        In the specified bucket or directory, select the objects or directories that you want to rename, click **More** \> **Rename**, and then enter the new name.

        **Note:** 

        -   You can only rename objects smaller than 1 GB.
        -   When you rename an object or a directory, the object or directory is copied, renamed, and then saved. The original object or directory is deleted. If you rename an object of the IA or Archive storage class and the storage period of the object does not reach the required value, fees incur for the advanced deletion.
    -   Delete objects/directories

        Select the object or directory that you want to delete, and then click **More** \> **Remove**.

        **Note:** If you delete an object of the IA or Archive storage class and the storage period of the object does not reach the required value, fees incur for the advanced deletion.

    -   Generate an access URL for an object.

        1.  Select the specified object, and then click **More** \> **Address**.
        2.  Enter the valid period of the URL, and then click **Generate**.
        3.  Click **Copy** or **Mail it** to send the URL to users who want to access the object. You can also scan the QR code to access the object.
    -   Preview an object.

        You can double-click an object to preview it. You can preview images and objects in the txt and pdf formats in ossbrowser.

    -   Manage parts.

        Select the specified bucket, and then click **Multipart**. You can delete unnecessary parts.


## More operations {#section_g45_w13_1hb .section}

-   Upload/Download performance optimization

    You can click **Settings** to configure the following parameters.

    -   **Upload tasks concurrent number**: Specify the maximum number of upload tasks that can be performed at the same time. If the number of upload tasks is larger than the value, the additional tasks are scheduled into a queue and wait for the current tasks to be complete. Setting this parameter properly based on your bandwidth can improve the upload speed.
    -   **Download tasks concurrent number**: Specify the maximum number of download tasks that can be performed at the same time. If the number of download tasks is larger than the value, the additional tasks are scheduled into a queue and wait for the current tasks to be complete. Setting this parameter properly based on your bandwidth can improve the upload speed.
    -   **overtime**: Specify the timeout period for tasks.
    -   **uploadpart size**: Specify the part size in multipart upload tasks. When the file to be uploaded is too large or the network condition is poor, you can set an appropriate part size to upload the object in multiple parts.
    -   **retry times**: Specify the allowed retry times in upload or download tasks.
-   Mail settings

    You can click **Settings** to set your E-mail account. All operations related to mails in ossbrowser are performed by the account.

-   Logging settings
    -   Enable the debug mode.

        You can enable the debug mode in the following two methods to view the logs generated by upload, download, and other operations.

        -   Method 1: Click **Settings**, and then click **Open debug**.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/155558042043798_en-US.png)

        -   Method 2: Continually click the OSS Browser logo at the upper left corner for 10 times.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/155558042043802_en-US.png)

    -   Enable logging.

        You can select whether to enable the logging function in the **Settings** dialog box.

        -   Select **Local log** to enable the local logging function to collect error logs. Logs collected by ossbrowser are stored in the following paths by default:
            -   Linux: ~/.config/oss-browser/log.log
            -   Mac: ~/Library/Logs/oss-browser/log.log
            -   Windows: %USERPROFILE%\\AppData\\Roaming\\oss-browser\\log.log
        -   Select **local info file log** to collect normal local file information.

