# Multipart upload {#concept_88434_zh .concept}

This topic describes how to use multipart upload.

To enable multipart upload, perform the following steps:

1.  Initialization \(using bucket.init\_multipart\_upload\): Obtain the Upload ID.
2.  Upload parts \(using bucket.upload\_part\): Upload data parts. You can upload multiple parts concurrently.
3.  Completing multipart upload \(using bucket.complete\_multipart\_upload\): After you have uploaded all parts, combine these parts into a complete object.

Run the following code for multipart upload:

```language-python
# -*- coding: utf-8 -*-
import os
from oss2 import SizedFileAdapter, determine_part_size
from oss2.models import PartInfo
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

key = '<yourObjectName>'
filename = '<yourLocalFile>'

total_size = os.path.getsize(filename)
# Use determine_part_size to determine the part size.
part_size = determine_part_size(total_size, preferred_size=100 * 1024)

# Initialize a multipart upload event.
upload_id = bucket.init_multipart_upload(key).upload_id
parts = []

# Upload parts one by one.
with open(filename, 'rb') as fileobj:
    part_number = 1
    offset = 0
    while offset < total_size:
        num_to_upload = min(part_size, total_size - offset)
		# The SizedFileAdapter(fileobj, size) method generates a new object, and re-calculates the initial append location.
        result = bucket.upload_part(key, upload_id, part_number,
                                    SizedFileAdapter(fileobj, num_to_upload))
        parts.append(PartInfo(part_number, result.etag))

        offset += num_to_upload
        part_number += 1

# Complete multipart upload.
bucket.complete_multipart_upload(key, upload_id, parts)

# Verify the multipart upload.
with open(filename, 'rb') as fileobj:
    assert bucket.get_object(key).read() == fileobj.read()

```

**Note:** We recommend you set a larger part size when the network connection is stable. Otherwise, set a smaller part size.

