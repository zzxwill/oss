# Copy objects {#concept_88465_zh .concept}

You can copy an object from a bucket \(source bucket\) to another bucket \(target bucket \) in the same region.

You must note the following limits before copy an object:

-   You must have the read and write permission on the source object.
-   You cannot copy an object from a region to another region. For example, you cannot copy an object from a bucket in Hangzhou region to a bucket in Qingdao region.

## Simple copy {#section_zsj_1tk_kfb .section}

You can use simple copy for objects smaller than 1 GB. Run the following code for simple copy:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourDestinationBucketName>')

bucket.copy_object('<yourSourceBucketName>', '<yourSourceObjectName>', '<yourDestinationObjectName>')

```

## Copy a large-sized object { .section}

To copy an object larger than 1 GB, you need to use UploadPartCopy. To enable UploadPartCopy, perform the following steps:

1.  Use bucket.init\_multipart\_upload to initiate an UploadPartCopy task.
2.  Start UploadPartCopy with bucket.upload\_part\_copy. Aside from the last part, all parts must be larger than 100 KB.
3.  Submit the UploadPartCopy task with bucket.complete\_multipart\_copy.

Run the following code for UploadPartCopy task

```language-python
# -*- coding: utf-8 -*-
import oss2
from oss2.models import PartInfo
from oss2 import determine_part_size

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

src_object = '<yourSourceObjectName>'
dst_object = '<yourDestinationObjectName>'

total_size = bucket.head_object(src_object).content_length
part_size = determine_part_size(total_size, preferred_size=100 * 1024)

# Initiate an UploadPartCopy task
upload_id = bucket.init_multipart_upload(dst_object).upload_id
parts = []

# Copy parts one by one.
part_number = 1
offset = 0
while offset < total_size:
    num_to_upload = min(part_size, total_size - offset)
    byte_range = (offset, offset + num_to_upload - 1)

    result = bucket.upload_part_copy(bucket.bucket_name, src_object, byte_range,dst_object, upload_id, part_number)
    parts.append(PartInfo(part_number, result.etag))

    offset += num_to_upload
    part_number += 1

# Complete the UploadPartCopy task.
bucket.complete_multipart_upload(dst_object, upload_id, parts)

```

For more information about UploadPartCopy, see [UploadPartCopy](../../../../reseller.en-US/API Reference/Multipart upload operations/UploadPartCopy.md#)

