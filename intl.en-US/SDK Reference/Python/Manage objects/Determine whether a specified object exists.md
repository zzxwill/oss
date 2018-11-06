# Determine whether a specified object exists {#concept_88454_zh .concept}

This topic describes how to determine whether a specified object exists.

Run the following code to determine whether a specified object exists:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

exist = bucket.object_exists('<yourObjectName>')
# If the returned value is true, it indicates that the specified object exists. If the returned value is false, it indicates that the specified object does not exist.
if exist:
	print('object exist')
else:
	print('object not eixst')

```

