# Range download {#concept_88443_zh .concept}

If you only need part of the data in an object, you can use range downloads to download specified content.

Run the following code for range download:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Obtain the data ranging from byte 0 to the 99th byte of the object (including byte 0 and the 99th byte , a total of 100 bytes). If the specified value range is invalid, for example, the specified range includes a negative number, or the specified value is larger than the object size, the entire object is downloaded.
object_stream = bucket.get_object('<yourObjectName>', byte_range=(0, 99))


```

