# Append upload {#concept_88432_zh .concept}

This topic describes how to use append upload.

Run the following code for append upload:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Set the position (the Position parameter) where the object is appended to 0.
result = bucket.append_object('<yourObjectName>', 0, 'content of first append')
# If the object is not uploaded for the first time, you can use the bucket.head_object method or the next_position attribute returned in the last append upload to get the append position.
bucket.append_object('<yourObjectName>', result.next_position, 'content of second append')

```

If the object already exists, exceptions occur in the following two sceanrios:

-   The file cannot be uploaded with append upload. In this case, an ObjectNotAppendable exception occurs.
-   The file can be uploaded with append upload, but the append position does not match the current file length. In this case, a PositionNotEqualToLength exception occurs.

