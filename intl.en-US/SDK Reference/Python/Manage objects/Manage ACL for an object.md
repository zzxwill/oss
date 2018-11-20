# Manage ACL for an object {#concept_88455_zh .concept}

The following table describes the permissions included in the Access Control List \(ACL\) for an object.

|Permission|Description|Value|
|:---------|:----------|:----|
|default|The ACL of an object is the same with that of its bucket.|oss2. OBJECT\_ACL\_DEFAULT|
|Private|Only the object owner and authorized users can read and write the object.|oss2. OBJECT\_ACL\_PRIVATE|
|Public read|Only the object owner and authorized users can read and write the object. Other users can only read the object. Authorize this permission with caution.|oss2. OBJECT\_ACL\_PUBLIC\_READ|
|Public read-write|All users can read and write the object. Authorize this permission with caution.|oss2. OBJECT\_ACL\_PUBLIC\_READ\_WRITE|

The ACL of objects take precedence over that of buckets. For example, if the ACL of a bucket is private, while the object ACL is public read-write, all users can read and write the object. If an object is not configured with an ACL, its ACL is the same as that of its bucket by default.

## Configure an ACL for an object {#section_h2j_xrk_kfb .section}

Run the following code to configure an ACL for an object:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Configure the ACL for the object.
bucket.put_object_acl('<yourObjectName>', oss2. OBJECT_ACL_PUBLIC_READ)

```

## Obtain the ACL for an object { .section}

You can run the following code to obtain the ACL for an object:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

print(bucket.get_object_acl('<yourObjectName>').acl)

```

