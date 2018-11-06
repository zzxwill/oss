# Streaming download {#concept_88441_zh .concept}

If you have a large object to download or it is time-consuming to download the entire object at a time, you can use streaming download. Streaming download enables you to download part of the object each time until you have downloaded the entire object.

Run the following code for streaming download:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
// This example uses the endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# The return value of bucket.get_object is a file-like object, which is also an iterable object.
object_stream = bucket.get_object('<yourObjectName>')
print(object_stream.read())

# The get_object interface returns a stream, which must be read by read() before being used to calculate the CRC checksum of the returned object data. Therefore, you must perform CRC verification after calling the interface.
if object_stream.client_crc ! = object_stream.server_crc:
    print "The CRC checksum between client and server is inconsistent!"

```

Run the following code to download data to a local file:

```language-python
import shutil

# object_stream is a file-like object. Therefore, you can use the shutil.copyfileobj method to download data to a local file.
object_stream = bucket.get_object('<yourObjectName>')
with open('<yourLocalFile>', 'wb') as local_fileobj:
    shutil.copyfileobj(object_stream, local_fileobj)

```

Run the following code to copy an object to another object using streaming copy.

```language-python

# object_stream is an iterable object, which can be copied to another object using streaming copy.
object_stream = bucket.get_object('<yourObjectName>')
bucket.put_object('<yourBackupObjectName>', object_stream)

```

