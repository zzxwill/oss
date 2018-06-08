# ossbrowser {#concept_xmg_h33_wdb .concept}

OSSBrowser is a graphical management tool from OSS, providing features similar to what Windows Explorer offers.  You can easily browse, upload, and download files, and perform resumable uploads or downloads.

OSSBrowser provides the following features: logging on by using an AccessKey or an authorization code, managing buckets and objects, providing policy authorization, and generating STS temporary authorization.

## Download and installation {#section_ppm_j33_wdb .section}

|Supported platform|Download address|
|:-----------------|:---------------|
|Window x32|[Window x32](https://github.com/aliyun/oss-browser/blob/master/all-releases.md)|
|Window x64|[Window x64](https://github.com/aliyun/oss-browser/blob/master/all-releases.md)|
|MAC|[MAC](https://github.com/aliyun/oss-browser/blob/master/all-releases.md)|
|Linux x64|[Linux x64](https://github.com/aliyun/oss-browser/blob/master/all-releases.md)|

## Log on to ossbrowser {#section_mq4_l33_wdb .section}

-   Log on to ossbrowser by using an AccessKey

    You can use an AK \(for example, a RAM account AK\) to log on to ossbrowser.

    **Note:** Do not use a primary account AK to log on to ossbrowser.

    1.  Log on to the [RAM console](https://ram.console.aliyun.com/) and create an RAM account.

        RAM accounts are divided into:

        -   RAM accounts with high permission \(that is, with all bucket permissions, and able to manage the sub-accounts configured by RAM \). The following configurations are recommended for beginners:

            ![](images/2994_en-US.png)

            **Note:** You can grant less permissions for RAM accounts. Refer to [Access control](../intl.en-US/Best Practice Guide/Access control/Overview.md#).

        -   RAM accounts with limited permission \(that is, RAM accounts with the permission to some buckets or sub directories\). We recommend that you use the [simple policy](#section_zyx_1k3_wdb) for authorization.
    2.  Log on to ossbrowser using a RAM account.

        ![](images/2995_en-US.png)

        **Note:** 

        -   Default OSS path \(optional \): the current AK only has the permission to a bucket or a path under the bucket.  You must preset a path in the Preset OSS Path box.
        -   Remember: select the Remember check box to save the AK. When you log on again, click **AK Histories** to select the key login without having to enter the AK manually. Do not select the check box in a temporarily used computer.
-   Log on to ossbrowser by using an authorization code

    You can use a temporary authorization code to log on to and use ossbrowser. Applicable scenario: You can provide a temporary authorization code to  appropriate people, allowing them to temporarily access a directory under your bucket before the authorization code expires. The temporary authorization code will automatically expire in due time.

    -   Generate temporary authorization token

        As an administrator, you first use AK to log on to ossbrowser, and in the administrative interface, select a file or directory that requires a temporary authorization to generate a temporary authorization token.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/3006_en-US.png)

    -   Use the authorization code for logon

        The temporary authorization token can be used to log on to ossbrowser before it expires, as shown in the following figure:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/3007_en-US.png)


## Manage a bucket {#section_idm_xj3_wdb .section}

After logging on to ossbrowser, you can manage the bucket, including:

-   Create a bucket
-   Delete a bucket
-   Modify bucket ACL
-   Manage fragments

## Managing files {#section_nm5_yj3_wdb .section}

The ossbrowser provides the following functions:

-   Adding, deleting, modifying, searching, copying of directories and files, and previewing files.

-   File transfer task management: upload \(supports drag and drop operation\), download, and resumable upload and download.

-   Address bar features: `oss://` protocol URL, browsing history, and saving bookmarks.

-   Management of Archive storage: creating Archive buckets and restoring Archive buckets.

    **Note:** All files in the Archive bucket are of the Archive storage class and need to be restored for access.


## Simplify policy authorization {#section_zyx_1k3_wdb .section}

1.  Select one or multiple target files and directories, and then select **Simple Policy**.

    ![](images/2998_en-US.png)

2.  Select Privileges in the Simplify policy authorization dialog box.
3.  View and copy the policy text. You can use the policy text to edit the policy of RAM accounts and RAM roles.

    ![](images/2999_en-US.png)

    You can also quickly authorize the target files or directories to a sub-account. \(The account must use an AK that has the permission to configure RAM.\)

    ![](images/3000_en-US.png)


## Generate STS temporary authorization {#section_e2m_nk3_wdb .section}

1.  Select a directory, and then select Authorization Token.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/3004_en-US.png)

2.  Set the parameters and click **Re-Generate** in the Generate Authorization Token dialog box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/3005_en-US.png)


## Related documents {#section_m3r_4l3_wdb .section}

-   [Create an AccessKey](https://www.alibabacloud.com/help/doc-detail/53045.htm)
-   [Access control](../intl.en-US/Best Practice Guide/Access control/Overview.md#)
-   [STS temporary authorization](../intl.en-US/Best Practice Guide/Access control/STS temporary access authorization.md#)
-   [Console-manage a bucket](../intl.en-US/Console User Guide/Manage buckets/Create a bucket.md#)
-   [Java SDK-manage a bucket](https://www.alibabacloud.com/help/doc-detail/32012.htm)
-   [API-create a bucket](../intl.en-US/API Reference/Bucket operations/PutBucket.md#)

