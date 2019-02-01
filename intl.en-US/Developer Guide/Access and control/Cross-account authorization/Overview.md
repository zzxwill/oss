# Overview {#concept_vvc_wmc_3gb .concept}

OSS provides multiple cross-account authorization methods to allow users using different accounts to access OSS resources.

The ACL for all OSS resources is private by default. The owner of a OSS resource can grant permissions to users using different accounts so that they can access the OSS resource. The following cross-account authorization methods can be used to allow other users to access OSS resources.

-   [Authorize a RAM user under another Alibaba Cloud account by adding a bucket policy](reseller.en-US/Developer Guide/Access and control/Cross-account authorization/Tutorial:Authorize a RAM user under another Alibaba Cloud account by adding a bucket policy.md#): A bucket policy authorize users based on resources. Compared with RAM policies, bucket policies can be easily configured in the graphical console. By configuring bucket policies, you can directly authorize other users so that they can access your bucket even you do not have permissions for RAM operations. You can configure bucket policies to grant bucket access permissions with IP address restrictions to anonymous users and RAM users under other accounts.

