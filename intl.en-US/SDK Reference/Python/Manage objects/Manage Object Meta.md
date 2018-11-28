# Manage Object Meta {#concept_88456_zh .concept}

This topic details how to manage Object Meta. For more information, see [Object Meta](../../../../reseller.en-US/Developer Guide/Manage files/Object Meta.md#) in the OSS Developer Guide.

## Configure HTTP headers {#section_tll_fsk_kfb .section}

OSS supports user-defined HTTP headers. Run the following code to configure HTTP headers:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

bucket.put_object('<yourObjectName>', '{"age": 1}', headers={'Content-Type': 'application/json; charset=utf-8'})

```

For more information about HTTP headers, see [RFC2616](https://tools.ietf.org/html/rfc2616).

## Configure user-defined Object Meta { .section}

You can define user-defined Object Meta for object descriptions. Run the following code to configure user-defined Object Meta:

```language-python
# -*- coding: utf-8 -*-

import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

bucket.put_object('', 'a novel', headers={'x-oss-meta-author': 'O. Henry', 'Content-Type': 'application/json; charset=utf-8'})
```

**Note:** Parameters with the x-oss-meta- prefix are used as the headers of user-defined Object Meta. For more information, see [PutObject](../../../../reseller.en-US/API Reference/Object operations/PutObject.md#section_fcx_vkw_bz).

## Modify Object Meta { .section}

Run the following code to modify Object Meta:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operation and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

bucket.update_object_meta('<yourObjectName>', {'x-oss-meta-author': 'O. Henry'})
# User-defined Object Meta is cleared each time the bucket.update_object_meta is called and must be configured again.
bucket.update_object_meta('<yourObjectName>', {'x-oss-meta-price': '100 dollar'})

```

Run the following code to change Object Meta, such as Content-Type:

```language-python
bucket.update_object_meta('<yourObjectName>', {'x-oss-meta-author': 'O. Henry'})
# User-defined Object Meta is cleared each time the bucket.update_object_meta is called and must be configured again.
bucket.update_object_meta('<yourObjectName>', {'Content-Type': 'text/plain'})

```

## Obtain Object Meta {#section_brj_s4w_mfb .section}

You can use the APIs provided by OSS Python SDK to obtain Object Meta.

|Method|Description|Advantage|
|:-----|:----------|:--------|
|get\_object\_meta|Obtains the ETag, Size \(object size\), and LastModified \(the time when the object is last modified\) of an object.|Simpler and faster|
|head\_object|Obtains all Object Meta.|None|

Run the following code to obtain Object Meta:

```
# -*- coding: utf-8 -*-

import oss2

# We recommend that you do not log on with the AccessKey of an Alibaba Cloud account because the Alibaba Cloud account has permissions on all APIs in OSS. Instead, we recommend that you log on as an authorized RAM user to access APIs or perform routine operation and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint China East 1 (Hangzhou). In actual scenarios, specify your required endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Obtain part of Object Meta.
simplifiedmeta = bucket.get_object_meta("<yourObjectName>")
print(simplifiedmeta.headers['Last-Modified']) 
print(simplifiedmeta.headers['Content-Length']) 
print(simplifiedmeta.headers['ETag']) 

# Obtain all Object Meta.
objectmeta = bucket.head_object("<yourObjectName>")
print(objectmeta.headers['Content-Type']) 
print(objectmeta.headers['Last-Modified']) 
print(objectmeta.headers['x-oss-object-type'])

```

