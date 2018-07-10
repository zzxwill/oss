# Upload an object {#task_zx1_4p4_tdb .task}

After a bucket is created, you can upload any type of objects to it.

A bucket is created. For more information, see [EN-US\_TP\_4333.md\#](intl.en-US/Quick Start/Create a bucket.md#).

You can upload objects in any of the following ways:

-   Use the OSS console to upload files smaller than 5 GB. See the following steps.
-   Use SDKs or APIs to upload files larger than 5 GB. For more information, see[Multipart upload](../../../../intl.en-US/Developer Guide/Upload files/Multipart upload.md#).
-   Use the graphical management tool ossbrowser to upload files. For more information, see [ossbrowser](../../../../intl.en-US/Utilities/ossbrowser.md#).

1.   Log on to the [OSS console](https://oss.console.aliyun.com/). 
2.   In the bucket name list, click the name of the bucket that you want to upload an object to. 
3.   Click the Files tab. 
4.   Click **Upload**. 

    **Note:** You can upload a file to a specified folder or the default folder. By clicking **Create Directory**, you can upload a file to a specified folder. By directly clicking **Upload**, you can upload a file to the OSS default folder.

5.   In the Directory Address box, set the path under which the file is uploaded to OSS. 
    -   Current Directory: The default path for file uploading. You cannot change the path if selecting this option.
    -   Specify Directory: If you want to upload a file to a certain folder, you must enter the path name. OSS automatically creates the directory and uploads the file to the directory.

        **Note:** For more information about a folder, see [Create a folder](../../../../intl.en-US/Console User Guide/Manage objects/Create a folder.md#).

6.  In the File ACL area, select the read/write permissions of the file. The read/write permissions of the bucket where the file belongs are inherited by default. 
7.  In the Upload area, drag the file to be uploaded to this area, or click **upload them directly** to select the file to be uploaded. 

    The Upload Task dialog box is displayed, indicating the upload progress. You can also see the upload progress by clicking **Upload Task** at the bottom left.

    **Note:** If the uploaded file has the same name as the existing file in the bucket, the existing file will be overwritten.


