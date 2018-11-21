# Anti-leech {#concept_32038_zh .concept}

To prevent your data on OSS from being leeched, OSS supports anti-leeching through the referer field settings in the HTTP header, including the following parameters:

-   Referer whitelist: Used to allow access only for specified domains to OSS data.
-   Empty referer: Determines whether the referer can be empty. If it is not allowed, only requests with the referer filed in their HTTP or HTTPS headers can access OSS data.

For more information about anti-leaching, see [Anti-leeching settings](../../../../reseller.en-US/Developer Guide/Security management/Anti-leech settings.md#) in the OSS Developer Guide.

## Configure the referer whitelist {#section_iyz_chn_kfb .section}

Run the following code to configure the referer whitelist:

```language-python
# -*- coding: utf-8 -*-
import oss2
from oss2.models import BucketReferer

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Configure to allow empty Referers (True indicates that an empty Referer is allowed, and False indicates that an empty Referer is not allowed), and configure the referer whitelist.
bucket.put_bucket_referer(BucketReferer(True, ['http://aliyun.com', 'http://*.aliyuncs.com']))

```

## Obtain a referer whiltelist { .section}

Run the following code to obtain a referer whiltelist:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

config = bucket.get_bucket_referer()
print('allow empty referer={0}, referers={1}'.format(config.allow_empty_referer, config.referers))

```

## Clear a referer whitelist { .section}

Run the following code to clear a referer whitelist:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# You cannot clear a referer whitelist directly. To clear a referer whitelist, you need to create the rule that allows an empty referer field and replace the original rule with the new rule.
bucket.put_bucket_referer(BucketReferer(True, []))

```

