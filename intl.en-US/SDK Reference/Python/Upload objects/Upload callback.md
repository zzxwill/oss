# Upload callback {#concept_88437_zh .concept}

This topic describes how to use upload callback.

For the complete code of upload callback, see [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/object_callback.py).

Run the following code for upload callback:

```language-python
# -*- coding: utf-8 -*-
import json
import base64
import os
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Prepare callback parameters.
callback_dict = {}
# Configure the IP address of the server you want to send the callback request to, for example, http://oss-demo.aliyuncs.com:23450 or http://127.0.0.1:9090.
callback_dict['callbackUrl'] = 'http://oss-demo.aliyuncs.com:23450'
# Configure the value of the Host field carried in the callback request header, such as oss-cn-hangzhou.aliyuncs.com.
callback_dict['callbackHost'] = 'oss-cn-hangzhou.aliyuncs.com'
# Configure the value of the body field carried in the callback request.
callback_dict['callbackBody'] = 'filename=${object}&size=${size}&mimeType=${mimeType}'
# Configure Content-Type for the callback request.
callback_dict['callbackBodyType'] = 'application/x-www-form-urlencoded'
# The callback parameter is in the JSON format and Base64-encoded.
callback_param = json.dumps(callback_dict).strip()
base64_callback_body = base64.b64encode(callback_param)
# Encoded callback parameters are carried in the request header and are sent to OSS.
headers = {'x-oss-callback': base64_callback_body}

# Upload and callback.
result = bucket.put_object('<yourObjectName>', 'a'*1024*1024, headers)

```

The put\_object, put\_object\_from\_file, and complete\_multipart\_upload methods provide upload callback features. For more information about upload callback, see [Upload callback](../../../../reseller.en-US/Developer Guide/Upload files/Upload callback.md#) in the Development Guide.

