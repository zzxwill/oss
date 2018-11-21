# Delete objects {#concept_88463_zh .concept}

This topic describes how to delete objects.

**Warning:** Delete objects with caution because deleted objects cannot be recovered.

## Delete an object {#section_gdc_5sk_kfb .section}

Run the following code to delete a single object:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

bucket.delete_object('<yourObjectName>')

```

## Delete multiple objects { .section}

Run the following code to delete multiple objects simultaneously:

```language-python
# Delete 3 objects. You can delete a maximum of 1,000 objects simultaneously.
result = bucket.batch_delete_objects(['<yourObjectName-a>', '<yourObjectName-b>', '<yourObjectName-c>'])
# Print the names of deleted objects.
print('\n'.join(result.deleted_keys))

```

