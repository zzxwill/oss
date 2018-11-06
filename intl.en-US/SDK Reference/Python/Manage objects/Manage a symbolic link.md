# Manage a symbolic link {#concept_88468_zh .concept}

This topic describes how to manage a symbolic link.

## Create a symbolic link {#section_ytq_ztk_kfb .section}

A symbolic link is a special object that maps to an object similar to a shortcut used in Windows.

Run the following code to create a symbolic link:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

objectName = '<yourObjectName>'
symlink = "<yourSymlink>";

bucket.put_symlink(objectName, symlink)

```

For more information about symbolic links, see [PutSymlink](../../../../reseller.en-US/API Reference/Object operations/PutSymlink.md#).

## Obtain object content for a symbolic link { .section}

To obtain a symbolic link, you must have the read permission on it. Run the following code to obtain the object content for a symbolic link:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

symlink = "<yourSymlink>";

bucket.get_symlink(symlink)

```

For more information about symbolic links, see [GetSymlink](../../../../reseller.en-US/API Reference/Object operations/GetSymlink.md#).

