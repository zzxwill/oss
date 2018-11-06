# Log on to the OSS Console with an RAM Sub-account {#concept_r3h_c1y_5db .concept}

The Alibaba Cloud OSS console provides an intuitive operation interface. Along with the Alibaba Cloud account, you can also log on to the OSS console using a sub-account \(RAM user\).

Log on to the OSS console using a RAM sub-account as follows:

1.  Create a RAM user.
2.  Authorize a sub-account.
3.  Log on to the console with a sub-account.

## Create a RAM user {#section_tf2_lcy_5db .section}

Log on to the RAM console, and create a RAM user through **User Management** \> **New User**. For detailed procedure, see in the [Create a RAM user](../../../../intl.en-US/User Guide/Identities/User.md#).

## Authorize a sub-account {#section_gxr_gly_5db .section}

Log on to the RAM console, select the corresponding RAM user, and click **Authorize** for authorization. For detailed procedure, see [RAM Authorization Help Documentation](../../../../intl.en-US/User Guide/Authorization/Permissions and authorization policies.md#).

To make sure that the sub-account can use the OSS console features after logging on to the console, access permissions to MNS, CloudMonitor, and CDN are also required along with the related OSS permissions, as shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4737/15414878941495_en-US.PNG)

## Log on to the console with a sub-account {#section_c2h_bmy_5db .section}

Do the following to log on to the console with a sub-account:

1.  Log on to the RAM console, and click **User Management**.
2.  Select the corresponding RAM user, and click **Manage** to configure related information.
3.  Turn on **Enable Console Logon**.
4.  Log on to the RAM console, view your **RAM user logon link**, and click the link to log on.

For more information, see [RAM User Manual](../../../../intl.en-US/User Guide/Identities/User.md#).

