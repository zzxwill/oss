# Enable logging in Python SDK {#concept_nx3_tfj_wfb .concept}

OSS Python SDK provides a logging function to easily track problems. This function is disabled by default.

With this function, you can locate and collect log information about OSS operations and save the information as log files in local disks.

**Note:** The logging function is only available in OSS Python SDK 2.6.x and later.

The format of a log file is as follows:

```
<time><name><level><threadId><message>
```

Log levels can be arranged in the following order of severity: CRITICAL \> ERROR \> WARNING \> INFO \> DEBUG \> NOTSET.

**Note:** After you specify a log level, only log information of levels that are equal to or higher than the specified level is recorded in local log files. For example, if you specify the log level as INFO, log information of levels CRITICAL, ERROR, WARNING, and INFO, is recorded in log files.

## Enable logging in Python SDK {#section_xc2_nhj_wfb .section}

To enable the logging function in the OSS Python SDK, run the following code:

```
# -*- coding: utf-8 -*-

import os
import logging
import oss2
from itertools import islice

# Initialize information such as AccessKeyId, AccessKeySecret, and Endpoint.
# Replace <AccessKeyId>, <AccessKeySecret>, <Bucket>, and <Endpoint> with actual values.
access_key_id = '<AccessKeyId>'
access_key_secret = '<AccessKeySecret>'
bucket_name = '<Bucket>'
endpoint = '<Endpoint>'

log_file_path = "log.log"
# Enable the logging function.
oss2.set_file_logger(log_file_path, 'oss2', logging.INFO)

# Create a bucket.
bucket = oss2. Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)

# Traverse all objects in the bucket.
for b in islice(oss2. ObjectIterator(bucket), 10):
    print(b.key)
# Obtain the object meta.
object_meta = bucket.get_object_meta('object')
```

The recorded log information is as follows:

```
2018-11-20 16:37:46,325 oss2.api [INFO] 26716 : Init oss bucket, endpoint: oss-cn-shenzhen.aliyuncs.com, isCname: False, connect_timeout: None, app_name: , enabled_crc: True
2018-11-20 16:37:46,326 oss2.api [INFO] 26716 : Start to List objects, bucket: funrily-hello, prefix: , delimiter: , marker: , max-keys: 100
2018-11-20 16:37:46,430 oss2.api [INFO] 26716 : List objects done, req_id: 5BF3C7DA236B3A201CE645F7, status_code: 200
2018-11-20 16:37:46,430 oss2.api [INFO] 26716 : Start to get object metadata, bucket: bucket-hello, key: object
2018-11-20 16:37:46,437 oss2.api [ERROR] 26716 : Exception: {
    'status': 404, 
    'x-oss-request-id': '5BF3C7DA236B3A201CE64679', 
    'details': {
    	'HostId': 'bucket-hello.oss-cn-shenzhen.aliyuncs.com', 
        'Message': 'The specified key does not exist.', 
        'Code': 'NoSuchKey', 
        'RequestId': '5BF3C7DA236B3A201CE64679', 
        'Key': 'object'
    }
}
```

According to the preceding log information, all objects in a bucket are traversed when the bucket is initialized. If a requested object does not exist, a `NoSuchKey` error occurs.

