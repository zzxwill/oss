# List objects {#concept_88458_zh .concept}

Objects are listed alphabetically. You can use OSS Python SDK to list objects in different methods.

## Simple list {#section_fgd_ksk_kfb .section}

Run the following code to list 10 objects in a specified bucket.

```language-python
# -*- coding: utf-8 -*-
import oss2
from itertools import islice

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

for b in islice(oss2. ObjectIterator(bucket), 10):
    print(b.key)

```

## List objects by a specified prefix { .section}

Run the following code to list objects with a specified prefix:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# List objects by a specified prefix 100 objects are listed by default.
for obj in oss2. ObjectIterator(bucket, prefix = 'img-'):
    print(obj.key)

```

## List all objects in a bucket { .section}

Run the following code to list all objects in a bucket:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Set the delimiter parameter to a slash (/).
for obj in oss2. ObjectIterator(bucket, delimiter = '/'):
	# Determine whether the obj is a directory using the is_prefix method.
    if obj.is_prefix():  # directory
        print('directory: ' + obj.key)
    else:                # object
        Print ('file: '+ obj. Key)

```

