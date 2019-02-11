# Bucket policy {#concept_jrf_4tm_2gb .concept}

You can configure bucket policies to authorize users to access your OSS buckets. Compared with a RAM policy, bucket policies can be directly configured by the bucket owner on the console for access authorization.

Bucket policies are suitable for the following scenarios:

-   Authorize RAM users of other accounts to access your OSS resources.

    You can authorize RAM users of other accounts to access your OSS resources.

-   Authorize anonymous users to access your OSS resources using specific IP addresses or IP ranges.

    In some cases, you must authorize anonymous users to access OSS resources using specific IP addresses or IP ranges. For example, confidential documents of an enterprise are only allowed to be accessed within the enterprise but not in other regions. Previously, configuring RAM policies for every user was a tedious and complex task because of the potential for a large number of internal users. To resolve this issue, you can configure access policies with IP restrictions based on bucket policies to authorize a large number of users easily and efficiently.


For more information about the configuration methods of bucket policies and video tutorials, see [Use bucket policies to authorize other users to access OSS resources](../../../../../reseller.en-US/Console User Guide/Manage objects/Use bucket policies to authorize other users to access OSS resources.md#).

