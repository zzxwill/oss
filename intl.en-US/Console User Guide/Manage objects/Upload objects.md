# Upload objects {#concept_jft_vhg_vdb .concept}

After creating a bucket, you can upload objects of any type to the bucket.

## Before you begin: {#section_uys_2ny_dhb .section}

You must create a bucket before uploading objects to it. For more information, see [Create a bucket](reseller.en-US/Quick Start/Create a bucket.md#).

## About this task: {#section_v2q_kny_dhb .section}

You can upload objects in the following methods:

-   Upload objects smaller than 5 GB in the OSS console.
-   Upload objects larger than 5 GB in the multipart upload method by using OSS SDK or API. For more information, see [Multipart upload](../../../../../reseller.en-US/Developer Guide/Upload files/Multipart upload.md#).
-   Use the graphical tool ossbrowser to upload objects. For more information, see [ossbrowser](../../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#).

## Procedure {#section_jhz_mkg_vdb .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the bucket that you want to upload objects to.
3.  In the overview page of the bucket, click the **Files** tab.
4.  Click **Upload**.

    **Note:** You can upload a file to a specified folder or to a default folder. You can select [Create a folder](reseller.en-US/Console User Guide/Manage objects/Create a folder.md#) before clicking **Upload** to upload the file to a specified folder. You can also directly click **Upload** to upload a file to a default OSS folder.

5.  In the **Upload** dialog box, set the folder where you want to upload objects to in **Upload To**.
    -   **Current**: Objects are uploaded to the current folder.
    -   **Specified**: Objects are uploaded to the specified folder. You must enter the name of the specified folder. OSS automatically creates the specified folder and uploads the object to it automatically

        **Note:** For more information about folders, see [Create a folder](reseller.en-US/Console User Guide/Manage objects/Create a folder.md#).

6.  Select the ACL for the object to be uploaded in **File ACL**. You can select one of the following four ACLs, in which **Inherited from Bucket** is the default ACL.

    -   **Inherited from Bucket**: The ACL for the object is the same as the ACL for the bucket.
    -   **Private**: Only authorized users can access the object.
    -   **Public Read**: All users \(including anonymous users\) can read the object. Only authorized users can write the object.
    -   **Public Read/Write**: All users can read and write the object.
    For more information about ACL, see [Change object ACL](reseller.en-US/Console User Guide/Manage objects/Change object ACL.md#).

7.  Drag one or multiple objects that you want to upload to the **Upload** area, or click **click here to upload** to select the objects that you want to upload.

    **Note:** 

    -   If the name of the object that you want to upload is the same as that of an existing object, the existing object is overwritten.
    -   Do not refresh or close the upload page when objects are being uploaded. Otherwise, the upload tasks are interrupted and the upload object list is cleared.

