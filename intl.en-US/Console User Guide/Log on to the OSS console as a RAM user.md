# Log on to the OSS console as a RAM user {#concept_r3h_c1y_5db .concept}

You can log on to the OSS console as a RAM user created on the RAM console.

## Application scenarios {#section_zt3_qjw_dgb .section}

You can log on to the OSS console as a RAM user in the following scenarios:

-   You want to share some internal documents of your enterprise stored in a bucket created by your Alibaba Cloud account. You can create RAM users for your staff and grant appropriate permissions for the RAM users. The staff can log on to the OSS console as the RAM users to view the shared documents.
-   Some of your partners need to view some materials regularly. You can store the materials in a bucket, create RAM users for the partners and authorize the RAM users to access the bucket. In this way, the partners can log on to the OSS console as RAM users to view the materials regularly.
-   In development environment where Alibaba Cloud accounts are not suitable for testing, you can create RAM users for testing.
-   Other scenarios.

    Follow these steps to log on to the OSS console as a RAM user:

    1.  [Create a RAM user](#).
    2.  [Authorize the RAM user](#).
    3.  [Log on to the OSS console as a RAM user](#).

## Create a RAM user {#section_tf2_lcy_5db .section}

1.  Log on to the [RAM console](https://ram.console.aliyun.com).
2.  Click**Identity Management** \> **Users** \> **Create User** to create a RAM user.
3.  Input the information about the user you want to create and configure the **Access Mode** as needed. You can also click **Add User** to create multiple users at one time. For more information about the configuration method, see the Create a RAM user chapter in [RAM user guide](../../../../../reseller.en-US/User Guide/Identity management/User management/RAM users.md#).
4.  Click **OK** and then click **Back**.

## Authorize the RAM user {#section_gxr_gly_5db .section}

1.  Click User to view the [User List](https://ram.console.aliyun.com/users).
2.  Select the RAM user that you want to authorize, click **Grant Permission**.
3.  Grant permissions to the RAM user as needed. Some permission policies are provided. However, you can add permissions to the policies as needed. For more information, see [Policy management](../../../../../reseller.en-US/User Guide/Permission management/Policy management.md#).

    **Note:** 

    To make sure that you can use the functions in the OSS console after logging on as a RAM user, you must also grant the access permissions for MNS, CloudMonitor, and CDN to the RAM user.![RAM user](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4737/15519401531495_en-US.PNG)


## Log on to the OSS Console as a RAM user {#section_c2h_bmy_5db .section}

Log on to the OSS console using an RAM sub-account as follows:

1.  Log on to the [RAM console](https://ram.console.aliyun.com).
2.  In the Overview page, find the URL displayed in **RAM user logon**. Open the URL and log on as a RAM user.
3.  Perform operations in the [OSS console](https://oss.console.aliyun.com).

For more information, see [RAM user guide](../../../../../reseller.en-US/User Guide/Identity management/User management/RAM users.md#).

