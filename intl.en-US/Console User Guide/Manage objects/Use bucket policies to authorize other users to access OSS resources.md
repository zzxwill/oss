# Use bucket policies to authorize other users to access OSS resources {#concept_ahc_tx4_j2b .concept}

You can use bucket policies to authorize other users to access your OSS resources.

Compared with the [RAM policy](../../../../../reseller.en-US//Authorization management/Policy management.md#), bucket policies can be directly configured by the bucket owner on the graphical console for access authorization. You can use bucket policies in the following common scenarios:

-   Authorize RAM users of other accounts to access your OSS resources.

    You can authorize RAM users of other accounts to access your OSS resources.

-   Authorize anonymous users to access your OSS resources using specific IP addresses or IP ranges.

    In some cases, you must authorize anonymous users to access OSS resources using specific IP addresses or IP ranges. For example, confidential documents of an enterprise are only allowed to be accessed within the enterprise but not in other regions. It takes a lot of effort to configure RAM policies for every user because of the large number of internal users. In this case, you can configure access policies with IP restrictions based on bucket policies to authorize users easily and efficiently.


## Authorize RAM users of other accounts to access your OSS resources {#section_nbp_by4_j2b .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the name of the bucket you want to authorize the user to access.
3.  On the overview page of the bucket, click the Files tab, and then click **Authorize**.
4.  On the Authorize page, click **Authorize**.
5.  On the Authorize page, configure Applied To.
    -   Whole Bucket: The authorization policy applies to the whole bucket.
    -   Specified Resource: The authorization policy applies only to specified resources in the bucket. If you select this option, you must enter the path of the specified resources, such as abc/myphoto.png. If the policy applies to a directory, you must add an asterisk \(\*\) at the end of the path, such as abc/\*.
6.  For **Accounts**, select from the following options:

    -   **Sub Account**: Select a RAM user under the current account from the drop-down list to grant bucket access permission to it. To select this option, you must log on to the console with your Alibaba Cloud account or as a RAM user that has the management permission on the bucket and the ListUsers permission on the RAM console.
    -   **Other Account**: If you want to grant the bucket access permission to another account or your account does not have the ListUsers permission, enter the UID of the account to which you want to grant the permission.
    -   **Anonymous Account \(\*\)**: If you want to grant permissions to all users, you can select **Anonymous Account \(\*\)**.
    **Note:** To grant the ListUsers permission to a RAM user, see

    -   [Authorize RAM users](../../../../../reseller.en-US/Quick Start/Authorize RAM users.md#).
    -   The following code template can be used to grant the ListUsers permission to a RAM user.

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": [
                        "ram:ListUsers"
                    ],
                    "Resource": [
                        "*"
                    ],
                    "Condition": {}
                }
            ]
        }
        ```

7.  Configure Authorized Operation.
    -   Read-Only: Authorized users can view, list, and download the resources.
    -   Read/Write: Authorized users can read and write the resources.
    -   Any Operation: Authorized users can perform any operation on the resources.
    -   None \(Deny\): Authorized users cannot perform any operation on the resource.

        **Note:** If multiple bucket policies are configured for a user, the user's access is determined by the combination of these policies. However, the user cannot perform any operation if the authorized operation is configured to None \(Deny\) in any of the policies. For example, if the authorized operation for a user is configured to Read Only in a bucket policy and Read/Write in another, the user has the Read/Write access which is the combination of the Read Only and Read/Write access. If the authorized operation of the user is configured to None \(Deny\) in another policy, the user only has the None \(Deny\) access.

8.  \(Optional\) Configure Conditions to authorize the user to access OSS resources using only specified IP addresses. You can select IP is or IP is not to allow or prohibit IP addresses or IP ranges used to access OSS resources.
    -   You can specify an IP address or multiple IP addresses as the condition, such as 10.10.10.10. Separate multiple IP addresses with commas \(,\).
    -   You can also specify an IP range as the condition, such as 10.10.10.1/24.
9.  Click **OK**.

## Authorize anonymous users to access your OSS resources using specific IP addresses or IP ranges {#section_byf_gz4_j2b .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the name of the bucket you want to authorize the user to access.
3.  On the overview page of the bucket, click the Files tab, and then click **Authorize**.
4.  On the Authorize page, click **Authorize**.
5.  On the Authorize page, configure Applied To.
    -   Whole Bucket: The authorization policy applies to the whole bucket.
    -   Specified Resource: The authorization policy applies only to specified resources in the bucket. If you select this option, you must enter the path of the specified resources, such as abc/myphoto.png. If the policy applies to a directory, you must add an asterisk \(\*\) at the end of the path, such as abc/\*.
6.  In the Accounts field, select Anonymous User \(\*\).

    **Warning:** We strongly recommend that you configure IP address conditions if you authorize anonymous users to access OSS resources. If you do not configure IP address conditions, your resources can be accessed by any user.

7.  Configure Authorized Operation.
    -   Read-Only: Authorized users can view, list, and download the resources.
    -   Read/Write: Authorized users can read and write the resources.
    -   Any Operation: Authorized users can perform any operation on the resources.
    -   None \(Deny\): Authorized users cannot perform any operation on the resource.

        **Note:** If multiple bucket policies are configured for some users, the users' access is determined by the combination of these policies. However, the users cannot perform any operation if the authorized operation is configured to None \(Deny\) in any of the policies. For example, if the authorized operation for some users is configured to Read Only in a bucket policy and Read/Write in another, the users have the Read/Write access which is the combination of the Read Only and Read/Write access. If the authorized operation of the users is configured to None \(Deny\) in another policy, the users only have the None \(Deny\) access.

8.  Configure Conditions. You can select IP is or IP is not to allow or prohibit IP addresses or an IP range used to access OSS resources.
    -   You can specify an IP address or multiple IP addresses as the condition, such as 10.10.10.10. Separate multiple IP addresses with commas \(,\).
    -   You can also specify an IP range as the condition, such as 10.10.10.1/24.
9.  Click **OK**.

