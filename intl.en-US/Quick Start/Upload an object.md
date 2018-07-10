# Upload an object {#task_zx1_4p4_tdb .task}

After a bucket is created, you can upload any type of objects \(files\) to it.

A bucket is created. For more information, see [Create a bucket](intl.en-US/Quick Start/Create a bucket.md#).

You can upload objects in any of the following ways:

-   Use the OSS console to upload objects smaller than 5 GB. See the following steps.
-   Use SDKs or APIs to upload objects larger than 5 GB. For more information, see[Multipart upload](../../../../intl.en-US/Developer Guide/Upload files/Multipart upload.md#).
-   Use the graphical management tool ossbrowser to upload objects. For more information, see [ossbrowser](../../../../intl.en-US/Utilities/ossbrowser.md#).

1.   Log on to the [OSS console](https://oss.console.aliyun.com/). 
2.  In the bucket name list, click the name of the bucket that you want to upload files to. 
3.   Click the Files tab. 
4.   Click **Upload**. 

    **Note:** You can upload files to a specified folder or the default folder. By clicking **Create Directory**, you can upload files to a specified folder. By directly clicking **Upload**, you can upload files to the OSS default folder.

5.  In the Directory Address box, set the directory where the files are uploaded. 
    -   Current Directory: If you select this option, the files will be uploaded to the current directory.
    -   Specify Directory: If you select this option, you must specify a directory, and OSS will automatically create the corresponding folder and upload the files to the folder.

        **Note:** For more information about a folder, see [Create a folder](../../../../intl.en-US/Console User Guide/Manage objects/Create a folder.md#).

6.  In the File ACL area, select the read/write permission of the files to be uploaded. By default, the files inherit the read/write permission of the bucket where they belong. 
7.  In the Upload area, drag the folders or files to be uploaded to this area, or click **upload them directly** to select the files to be uploaded. 

    The Upload Task dialog box is displayed, indicating the upload progress. You can also see the upload progress by clicking **Upload Task** at the bottom left.

    **Note:** If the uploaded files have the same names as the existing files in the bucket, the existing files will be overwritten.


