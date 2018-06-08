# Upload an object {#task_zx1_4p4_tdb .task}

After a bucket is created, you can upload objects to it in any of the following ways:

-   Using the OSS console to upload files smaller than 5 GB.
-   Using SDKs or APIs to upload files larger than 5 GB. For more information, see[Multipart upload](../intl.en-US/Developer Guide/Upload files/Multipart upload.md#).
-   Using ossbrowser to upload files conveniently. For more information, see [ossbrowser](../intl.en-US/Utilities/ossbrowser.md#).

**Note:** If you upload files with the same names as existing files in the bucket, the existing files will be overwritten.

1.   Log on to the [OSS console](https://oss.console.aliyun.com/). 
2.   Click to open the target bucket. 
3.   Click Files tab. 
4.   Click **Upload**. 

    **Note:** You can upload a file to a specified folder or the default folder. By clicking **Create Directory** before clicking [Upload](../intl.en-US/Console User Guide/Manage objects/Create a folder.md#), you can upload a file to a specified folder.  By directly clicking **Upload**, you can upload a file to the OSS default folder.

5.   In the Directory Address box, set the path under which the file is uploaded to OSS. 
    -   Current Directory: The default path for file uploading. You cannot change the path if selecting this option.
    -   Specify Directory: If you want to upload a file to a certain folder, you must enter the path name. OSS automatically creates the directory and uploads the file to the directory.

        **Note:** For the description of and operations on a folder, see [Create a folder](../intl.en-US/Console User Guide/Manage objects/Create a folder.md#).

6.   In the  File ACL region, select the read/write permissions of the file.  The read/write permissions of the bucket where the file belongs are inherited by default. 
7.   In the Upload region, drag the file to be uploaded to this region, or click **upload them directly** to select the file to be uploaded. 
8.   In the Phone Verification dialog box, click **Get**, enter the received code, and click **OK** to open Upload tasks dialog box, showing the upload progress. You can also see the progress of the upload by clicking **Upload Tasks** at the bottom left. 

