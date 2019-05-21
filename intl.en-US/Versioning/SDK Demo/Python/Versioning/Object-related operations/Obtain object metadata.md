# Obtain object metadata {#concept_265545 .concept}

In a bucket with versioning enabled or suspended, the HeadObject operation only obtains the metadata of the current version of an object.

**Note:** If the current version of the target object is a delete marker, the 404 Not Found error is returned. You can specify the versionId to obtain the metadata of a specified version of the target object.

You can run the following code to obtain the metadata of an object:

``` {#codeblock_gej_1o2_nqa}
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Inputs the versionId field to the param parameter.
params = dict()
params['versionId'] = '<yourObjectVersionId>'

# Obtains a part of metadata of the object.
simplifiedmeta = bucket.get_object_meta("<yourObjectName>", params=params)
print(simplifiedmeta.headers['Last-Modified'])
print(simplifiedmeta.headers['Content-Length'])
print(simplifiedmeta.headers['ETag'])

# Obtains all metadata of the object.
objectmeta = bucket.head_object("<yourObjectName>", params=params)
print(objectmeta.headers['Content-Type'])
print(objectmeta.headers['Last-Modified'])
print(objectmeta.headers['x-oss-object-type'])
```

