# RAM user {#concept_wdd_hvk_2gb .concept}

With Alibaba Cloud RAM, you can create RAM users under your Alibaba Cloud account. Each RAM user has their own AccessKeys. In this case, your Alibaba Cloud account is referred to as the primary account and the created RAM users are referred to as the sub-accounts. The AccessKey of a sub-account can be used only to perform operations authorized by your Alibaba Cloud account and use resources authorized by your Alibaba Cloud account.

## Scenario {#section_rkf_xxk_2gb .section}

If multiple users need to use resources under your Alibaba Cloud account, they can only use the AccessKey of your Alibaba Cloud account to access the resources. If this occurs, the following two issues arise:

-   Your AccessKey is exposed to multiple users, which increases the risk of mistakenly exposing its contents.
-   You cannot control which user or users can access specific resources \(such as buckets\).

To resolve the preceding issues, you can use Alibaba Cloud RAM to create RAM users with their own AccessKeys under your Alibaba Cloud account. In this case, your Alibaba Cloud account is referred to as the primary account and the created RAM users are referred to as the sub-accounts. You can perform only operations The AccessKey of a sub-account can be used only to perform operations authorized by your Alibaba Cloud account and use resources authorized by your Alibaba Cloud account.

## Implementation {#section_ux5_1yk_2gb .section}

For more information about RAM and how to create a RAM user, see [Introduction](../../../../../reseller.en-US/User Guide/Overview.md#). To grant OSS access permissions to users by creating RAM policies, see [RAM Policy](reseller.en-US/Developer Guide/Access and control/Access control based on RAM Policy/RAM policy.md#).

