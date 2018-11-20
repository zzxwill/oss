# Permission management {#concept_c3k_mvr_gfb .concept}

## Log on to ossbrowser with a RAM user account {#section_zqj_4vr_gfb .section}

To ensure data security, we recommend that you log on to ossbrowser by using the AccessKey \(AK\) of a RAM user account \(sub-account\). To log on to ossbrowser, perform the following steps:

1.  Create a RAM user account and an AccessKey. For more information, see [Create a RAM user](../../../../reseller.en-US/Quick Start/Create a RAM user.md#).

    RAM user accounts can be classified into two types based on their permissions:

    -   RAM user accounts with high-level permissions \(can access all buckets and can manage the sub-accounts configured by RAM\). For new users, we recommend that you use the following configurations:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/15427003766324_en-US.png)

    -   RAM user accounts with limited permissions \(can access only some buckets or sub-directories\). For new users, we recommend that you [Grant permissions with a simple policy](#section_zyx_1k3_wdb).

        **Note:** You can authorize RAM user accounts with lower permissions. For details, see [Access control](../../../../reseller.en-US/Best Practices/Access control/Overview.md#).

2.  Configure the following options to log on to ossbrowser:
    -   **Endpoint**: Use the default value.
    -   **AccessKeyId** and **AccessKey Secret**: Enter the AccessKey of the RAM account.
    -   **Preset OSS Path**:
        -   RAM accounts with high permissions: No configuration is required.
        -   RAM accounts with limited permissions: Configuration is required. Enter the name of the OSS bucket or sub-directory that you want to access \(the RAM account must have the permission on the path\). The format of the path is as follows: **oss:// bucket name/sub-directory name/**.
    -   **Remember**: Select to save the AccessKey. When you log on the ossbrowser later, you can simply click **AK Histories** and select the saved AccesKey instead of entering the AccessKey repeatedly. Do not select this option if you are using a computer temporarily.

## Log on to ossbrowser with a temporary authorization code {#section_nwj_fwr_gfb .section}

You can use a temporary authorization code to log on to ossbrowser. You can provide a temporary authorization code to authorized users, allowing them to access a directory under your bucket temporarily before the authorization code expires. The temporary authorization code automatically becomes invalid after it expires.

1.  Generate a temporary authorization code

    Use the AccessKey of the primary account to log on to ossbrowser as the administrator. Select the object or directory to be accessed temporarily by the authorized users, and generate a temporary authorization code, as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/15427003766326_en-US.png)

2.  Log on to ossbrowser with the authorization code

    The temporarily authorized users can use the authorization code to log on to ossbrowser before it expires, as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/15427003766327_en-US.png)


## Grant permissions with a simple policy {#section_zyx_1k3_wdb .section}

1.  Select one or more objects or directories to be accessed temporarily by the authorized users and then click **Simple Policy**, as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/15427003766328_en-US.png)

2.  On the Simplify policy authorization dialog box, select Privileges.
3.  View and copy the policy text. You can use the policy text to edit the policy of RAM accounts and RAM roles.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/15427003766329_en-US.png)

    You can also grant permissions to sub-accounts on this page. The current AccessKey used to log on must have the permission to configure RAM.


