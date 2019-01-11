# Upload an object {#task_zx1_4p4_tdb .task}

After a bucket is created, you can upload any type of object \(file\) to it.

A bucket is created. For more information, see [Create a bucket](reseller.en-US/Quick Start/Create a bucket.md#).

You can upload an object in any of the following ways:

-   Use the OSS console to upload an object smaller than 5 GB. See the following steps.
-   Use SDKs or APIs to upload an object larger than 5 GB. For more information, see[Multipart upload](../../../../../reseller.en-US/Developer Guide/Upload files/Multipart upload.md#).
-   Use the graphical management tool ossbrowser to upload an object. For more information, see [ossbrowser](../../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#).

1.   Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss). 
2.  In the bucket name list, click the name of the bucket that you want to upload a file to. 
3.   Click the Files tab. 
4.   Click **Upload**. 
5.  In the Directory Address box, set the directory where the file is uploaded. 
    -   Current Directory: If you select this option, the file will be uploaded to the current directory.
    -   Specify Directory: If you select this option, you must specify a directory, and OSS will automatically create the corresponding folder and upload the file to the folder.

        **Note:** For more information about folders, see [Create a folder](../../../../../reseller.en-US/Console User Guide/Manage objects/Create a folder.md#).

6.  In the File ACL area, select the read/write permission of the file to be uploaded. By default, a file inherits the read/write permission of the bucket where it belongs. 
7.  In the Upload area, drag one or more files to be uploaded to this area, or click **upload them directly** to select one or more files to be uploaded. 

    **Note:** 

    -   If the uploaded file has the same name as an existing file in the bucket, the original file will be overwritten.
    -   When uploading one or more files, do not refresh or close the page. Otherwise, the upload task is interrupted and the upload list is cleared.

