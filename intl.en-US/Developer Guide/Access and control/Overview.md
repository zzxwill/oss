# Overview {#concept_e4s_mhv_tdb .concept}

OSS provides multiple access control methods, including ACLs, RAM policies, and bucket policies, for users who access objects stored in buckets.

-   [ACL](reseller.en-US/Developer Guide/Access and control/ACL.md#): OSS provides Access Control List \(ACL\) for access control. An ACL is set based on resources. You can set ACLs for buckets or objects. You can set an ACL for a bucket when you create the bucket or for an object when you upload the object to OSS. You can also modify the ACL for a created bucket or an uploaded object at anytime.

-   [RAM Policy](reseller.en-US/Developer Guide/Access and control/Access control based on RAM Policy/RAM policy.md#): Resource Access Management \(RAM\) is a service provided by Alibaba Cloud for resource access control. RAM policies are configured based on users. By configuring RAM policies, you can manage multiple users in a centralized manner and control the resources that can be accessed by the users. For example, you can control the permission of a user so that the user can only read a specified bucket. A RAM user belongs to the Alibaba Cloud account under which it was created, and does not own any actual resources. That is, all resources belong to the corresponding Alibaba Cloud account.

-   [Bucket Policy](reseller.en-US/Developer Guide/Access and control/Bucket policy.md#): Bucket policies are configured based on resources. Compared with RAM policies, bucket policies can be directly configured on the graphical console. By configuring bucket policies, you can authorize users to access your bucket even you do not have permissions for RAM operations. By configuring bucket policies, you can grant access permissions to RAM users under other Alibaba Cloud accounts, and to anonymous users who access your resources from specified IP addresses or IP ranges.


