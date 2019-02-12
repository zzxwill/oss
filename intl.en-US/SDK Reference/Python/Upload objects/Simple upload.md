# Simple upload {#concept_88426_zh .concept}

In simple uploads, you can upload files of the following five types: strings, bytes, Unicode, local files, and file streams.

You can use the bucket.put\_object method to upload a file. This method supports multiple types of inputs, which are listed in the following table.

|Type|Upload method|
|:---|:------------|
|String|Direct uploaded|
|Bytes|Direct uploaded|
|Unicode|Uploaded after being converted to bytes encoded in UTF-8.|
|Local files|Uploaded as File objects, which must be opened in the binary mode.|
|File stream|Uploaded as Iterable objects in Chunked Encoding method.|

**Note:** If you upload an object whose name is the same as that of an existing object that you can access, the existing object is overwritten by the uploaded object, and a 200 OK message is returned.

For more information about PutObject, see [Detail analysis](../../../../../reseller.en-US/API Reference/Object operations/PutObject.md#section_fcx_vkw_bz).

## Upload a string {#section_iwr_bcw_mfb .section}

You can run the following code to upload a string:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Return value
result = bucket.put_object('<yourObjectName>', 'content of object')
# HTTP return code
print('http status: {0}'.format(result.status))
# Request ID The request ID is the unique identification of a request. We strongly recommend you to add this parameter in the program log.
print('request_id: {0}'.format(result.request_id))
# Etag is a unique attribute of the value returned by put_object.
print('ETag: {0}'.format(result.etag))
# HTTP response header
print('date: {0}'.format(result.headers['date']))

```

## Upload bytes {#section_vdq_dcw_mfb .section}

You can run the following code to upload bytes:

```language-python
# -*- coding: utf-8 -*-

import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

bucket.put_object('<yourObjectName>', b'content of object')


```

## Upload Unicode {#section_i2w_fcw_mfb .section}

You can run the following code to upload Unicode:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

bucket.put_object('<yourObjectName>', u'content of object')

```

## Upload local files {#section_d14_3cw_mfb .section}

You can run the following code to upload a local file:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Files must be opened in binary mode because the number of bytes included in the file is required for upload.
with open('<yourLocalFile>', 'rb') as fileobj:
    # The seek method specifies that the read or write operations start from the 1000th byte. The upload starts from the 1000th byte until the end of the file.
    fileobj.seek(1000, os.SEEK_SET)
    # The tell method is used to return to the current location.
    current = fileobj.tell()
    bucket.put_object('<yourObjectName>', fileobj)

```

OSS Python SDK provides a more convenient method to upload local files:

```language-python
bucket.put_object_from_file('<yourObjectName>', '<yourLocalFile>')

```

## Upload a file stream {#section_qtj_jcw_mfb .section}

You can run the following code to upload a file stream:

```language-python
# -*- coding: utf-8 -*-
import oss2
import requests

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# The requests.get method returns an Iterable object, and the Python SDK uploads the file stream in Chunked Encoding method.
input = requests.get('http://www.aliyun.com')
bucket.put_object('<yourObjectName>', input)

```

