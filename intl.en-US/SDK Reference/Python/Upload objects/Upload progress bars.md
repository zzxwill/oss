# Upload progress bars {#concept_dfm_s2j_kfb .concept}

You can use progress bars to indicate the upload or download progress.

For the complete code of progress bars, see [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/object_progress.py).

The following code is used as an example to describe how to view progress information with bucket.put\_object:

```
# -*- coding: utf-8 -*-
from __future__ import print_function
import os, sys
import oss2
# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
# If the length of the data to be uploaded cannot be determined, the value of total_bytes is None.
def percentage(consumed_bytes, total_bytes):
    if total_bytes:
        rate = int(100 * (float(consumed_bytes) / float(total_bytes)))
        print('\r{0}% '.format(rate), end='')
        sys.stdout.flush()
# progress_callback is an optional parameter used to implement progress bars.
bucket.put_object('<yourObjectName>', 'a'*1024*1024, progress_callback=percentage)
```

