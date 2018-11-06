# Authorized access {#concept_32033_zh .concept}

This topic describes how to authorize access to users.

## Sign a URL to authorize temporary access {#section_zx1_55k_kfb .section}

You can provide a signed URL to a visitor for temporary access. When you sign a URL, you can specify the expiration time for a URL to restrict the period of access from visitors.

Run the following code to download objects with a signed URL:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Set the expiration time of the  URL to one hour.
print(bucket.sign_url('GET', '<yourObjectName>', 60))

```

## Use STS for temporary access authorization { .section}

For the complete code of using STS, see [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/sts.py).

You can use Security Token Service \(STS\) for temporary access authorization. For more information about STS, see [Introduction](../../../../reseller.en-US/API Reference/OSS API documentation overview.md#) in the access control API \(STS\) reference. For more information about accounts and authorization, see [STS temprory access authorization](../../../../reseller.en-US/Best Practices/Access control/STS temporary access authorization.md#) in best practices.

You must install the official Python STS client first:

```language-bash
pip install aliyun-python-sdk-sts

```

Please use version 2.0.6 and later. Run the following code to upload files with STS temporary authorization:

```language-python
# -*- coding: utf-8 -*-

from aliyunsdkcore import client
from aliyunsdksts.request.v20150401 import AssumeRoleRequest
import json
import oss2

# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
endpoint = 'oss-cn-hangzhou.aliyuncs.com'
# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
access_key_id = '<yourAccessKeyId>'
access_key_secret = '<yourAccessKeySecret>'
bucket_name = '<yourBucketName>'
# role_arn is the resource name of the role.
role_arn = '<yourRoleArn>'

clt = client.AcsClient(access_key_id, access_key_secret, 'cn-hangzhou')
req = AssumeRoleRequest.AssumeRoleRequest()

# Set the format of returned values to JSON.
req.set_accept_format('json')
req.set_RoleArn(role_arn)
req.set_RoleSessionName('session-name')
body = clt.do_action(req)

# Use the AccessKeyId and AccessKeySecret to apply for a temporary token from STS.
token = json.loads(body)

# Use the authentication information in the temporary token to initialize the StsAuth instance.
auth = oss2. StsAuth(token['Credentials']['AccessKeyId'],
                    token['Credentials']['AccessKeySecret'],
                    token['Credentials']['SecurityToken'])

# Use the StsAuth instance to initialize the bucket.
bucket = oss2. Bucket(auth, endpoint, bucket_name)

# Upload a string.
bucket.put_object('object-name.txt', b'hello world')

```

