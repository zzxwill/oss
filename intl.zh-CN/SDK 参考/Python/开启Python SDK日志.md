# 开启Python SDK日志 {#concept_nx3_tfj_wfb .concept}

为方便追查问题，Python SDK提供了日志记录功能，该功能默认处于关闭状态。

Python SDK日志记录功能可以收集定位各类OSS操作的日志信息，并以日志文件的形式存储在本地。

**说明：** 开启Python SDK日志记录功能需要 Python SDK 2.6.x 以上版本。

日志文件的格式为：

```
<time><name><level><threadId><message>
```

日志级别：CRITICAL \> ERROR \> WARNING \> INFO \> DEBUG \> NOTSET

**说明：** 指定日志级别后，本地日志文件只会记录指定级别及高于指定级别的信息。例如，指定日志级别为INFO，则日志文件会记录CRITICAL 、ERROR、WARNING及INFO级别的相关信息。

## 开启Python SDK日志记录功能 {#section_xc2_nhj_wfb .section}

以下代码用于开启Python SDK日志记录功能：

```
# -*- coding: utf-8 -*-

import os
import logging
import oss2
from itertools import islice

# 初始化AccessKeyId、AccessKeySecret、Endpoint等信息。
# 请将<AccessKeyId>、<AccessKeySecret>、<Bucket>及<访问域名>分别替换成对应的具体信息。
access_key_id = '<AccessKeyId>'
access_key_secret = '<AccessKeySecret>'
bucket_name = '<Bucket>'
endpoint = '<访问域名>'

log_file_path = "log.log"
# 开启日志
oss2.set_file_logger(log_file_path, 'oss2', logging.INFO)

# 创建Bucket对象。
bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)

# 遍历文件目录
for b in islice(oss2.ObjectIterator(bucket), 10):
    print(b.key)
# 获取文件元信息
object_meta = bucket.get_object_meta('object')
```

相应的日志信息如下：

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

由以上日志信息可知，初始化一个Bucket对象会遍历其中所有的Object，当请求的Object不存在时会发生错误，错误信息为：`NoSuchKey`。

