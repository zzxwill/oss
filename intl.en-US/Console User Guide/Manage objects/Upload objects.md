# Upload objects {#concept_jft_vhg_vdb .concept}

After you create a bucket, you can upload objects \(files\) to the bucket in either of the following ways:

-   You can upload the object smaller than 5 GB by using the OSS console.

-   You can upload the object larger than 5 GB by using SDKs or APIs. For more information, see [Introduction](../../../../intl.en-US/API Reference/Multipart upload operations/Introduction.md#).


**Note:** If the name of the object to be uploaded is duplicate with that of the existing one in the bucket, it overwrites the existing one.

## Procedure {#section_jhz_mkg_vdb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  Click the name of the bucket which you want to upload objects to.
3.  Click the **Files** tab.
4.  Click **Upload** to open the Upload box.

    **Note:** You can upload a file to a specified folder or to a default folder. You can select [Create a folder](intl.en-US/Console User Guide/Manage objects/Create a folder.md#) before clicking **Upload** to upload the file to a specified folder. You can also directly click **Upload** to upload a file to a default OSS folder.

5.  In the **Directory Address** box, set the directory for the objects to be uploaded.
    -   Current Directory: If you select this option, the objects will be uploaded to the current directory.
    -   Specify Directory: If you select this option, enter the directory such as \*\*photos\\\*\*. Then OSS will automatically create a folder named \*\*photos\\\*\* and upload the objects to it.

        **Note:** You can also create a folder manually. For more information, see [Create a folder](intl.en-US/Console User Guide/Manage objects/Create a folder.md#).

6.  In the **File ACL** area, select the read/write permissions of the objects to be uploaded. - Inherited from Bucket: By default, the read/write permissions of the objects are inherited from the bucket which the objects are uploaded to. - Private: Only the owner of the bucket and the authorized users can perform read, write, and delete operations on the objects. Other users cannot access the objects. - Public Read: Only the owner of the bucket and the authorized users can perform write and delete operations on the objects. Anyone \(including anonymous access\) can read the objects. - Public Read/Write: Anyone \(including anonymous access\) can read, write, and delete the objects. The fees incurred by such operations are borne by the owner of the bucket. Use this permission with caution.
7.  Drag one or multiple objects to be uploaded to the **Upload** area, or click **upload them directly** to select the objects to be uploaded.
8.  Object names must comply with the naming conventions: - Object names must use UTF-8 encoding. - Object names must be at least 1 byte and no more than 1023 bytes in length. - Object names cannot start with a backslash \( / \) or a forward slash \( \\ \). - Object names are case sensitive.

