# Permission management {#concept_c3k_mvr_gfb .concept}

## Log on to ossbrowser as a RAM user {#section_zqj_4vr_gfb .section}

To ensure data security, we recommend that you log on to ossbrowser by using the AccessKey \(AK\) of a RAM user. To log on to ossbrowser, follow these steps:

1.  Create a RAM user and an AK. For more information, see [Create a RAM user](../../../../reseller.en-US/Quick Start/Create a RAM user.md#).

    RAM users can be classified into two types based on their permissions:

    -   Administrator RAM user: Indicates a RAM user with administration permissions. For example, a RAM user that can manage all buckets and authorize other RAM users is an administrator RAM user. You can log on to the RAM console with your Alibaba Cloud account to create an administrator RAM user and grant permissions to the administrator RAM user, as shown in the following figure.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/15445769876324_en-US.png)

    -   Operator RAM user: Indicates a RAM user that only has the read-only permission on a bucket or a directory. The administrator can [Grant permissions with a simple policy](#section_zyx_1k3_wdb) to authorize a RAM user.

        **Note:** You can grant lower-level permissions to RAM users. For details, see [Access control](../../../../reseller.en-US/Best Practices/Access control/Overview.md#).

2.  Set the following parameters to log on to ossbrowser:
    -   **Endpoint**: Use the default value.
    -   **AccessKeyId** and **AccessKeySecret**: Enter the AK of the RAM user.
    -   **Preset OSS Path**:
        -   Administrator RAM users with administration permissions on all buckets: No configuration is required.
        -   Operator RAM users: Configurations are required. Enter the path of the OSS bucket or sub-directory that you want to access \(the RAM user must have permission to access the OSS bucket or sub-directory\). The path format is as follows: **oss:// bucket name/sub-directory name/**.
    -   **Remember**: Select to save the AK. When you log on to ossbrowser later, you can simply click **AK Histories** and select the saved AK instead of entering the AK repeatedly. Do not select this option if you use a shared computer.
    -   ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21830/154457698733074_en-US.png)

## Log on to ossbrowser with a temporary authorization code {#section_nwj_fwr_gfb .section}

You can use a temporary authorization code to log on to ossbrowser. You can provide authorized users with a temporary authorization code to allow them to access a directory under your bucket temporarily before the authorization code expires. The temporary authorization code automatically becomes invalid after it expires.

1.  Generate a temporary authorization code.

    Use the AK of an administrator RAM user to log on to ossbrowser. Select the object or directory to be accessed temporarily by the authorized users, and generate a temporary authorization code, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/15445769876326_en-US.png)

2.  Log on to ossbrowser with the authorization code.

    The temporarily authorized users can use the authorization code to log on to ossbrowser before it expires, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/15445769876327_en-US.png)


## Grant permissions with a simple policy {#section_zyx_1k3_wdb .section}

After logging on to ossbrowser as an administrator RAM user, you can **Grant permissions with a simple policy** to create an operator RAM user, or grant an operator RAM user the read-only or read/write permission on a bucket or a directory.

**Note:** Alibaba Cloud ossbrowser provides simple policy authorization, which is an access control feature based on the Alibaba Cloud RAM service. You can also log on to the RAM console through the official website of Alibaba Cloud to manage your RAM user more precisely.

1.  Select one or more objects or directories to be accessed temporarily by the authorized users and then click **Simple Policy**, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/15445769886328_en-US.png)

2.  On the **Simplify policy authorization** dialog box, select **Privileges**.
3.  You can also grant permissions to an existing operator RAM user or create a new operator RAM user in this dialog box.

    **Note:** To use simple policy authorization, you must log on to ossbrowser by using the AK of an RAM user that has the RAM configuration permission, for example, the AK of an administrator RAM user that has the RAM configuration permission.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4906/15445769886329_en-US.png)

    The policy is generated in text. You can view, copy, and use the text as needed. For example, you can copy the policy text and use it to edit the authorization rules for RAM users and roles in the RAM console.


