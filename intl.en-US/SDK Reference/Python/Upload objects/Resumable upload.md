# Resumable upload {#concept_edl_r2j_kfb .concept}

You can use resumable upload to split a file you want to upload into several parts and upload them simultaneously. After you have uploaded all parts, you can combine these parts into a complete object. Thus, you can complete uploading the entire object.

You can use the oss2.resumable\_upload method to upload a specified file with resumable upload, You can configure the following parameters.

|Parameter|Description|Required or not|Default value|
|:--------|:----------|:--------------|:------------|
|bucket|Specifies the bucket name.|Required|None|
|key|Specifies the object name.|Required|None|
|filename|Specifies the name of the local file to upload.|Required|None|
|store|Specifies the directory where the progress infomration is stored.|Optional|Directory '.py-oss-upload' under the HOME directory|
|headers|Specifies the HTTP header.|Optional|None|
|multipart\_threshold|Specifies a object length threshold. If the object length is larger than the threshold, the resumable upload is used.|Optional|10 MB|
|part\_size|Specifies the part size.|Optional|Auto-calculated|
|progress\_callback|Specifies the upload progress callback function.|Optional|None|
|num\_threads|Specifies the number of concurrent upload threads.|Optional|1|

Run the following code for resumable upload:

```
# -*- coding: utf-8 -*-
import oss2
# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
# When the file length is larger than or be equal to the value of the optional parameter multipart_threshold (10 MB by default), the resumable upload is used. If you do not use the store parameter to specify the directory, a py-oss-upload directory is created under the HOME directory to save upload progress information.
oss2.resumable_upload(bucket, '<yourObjectName>', '<yourLocalFile>')
```

Python SDK 2.1.0 and later support resumable upload with optional parameters, as shown in the following code:

```
# If you use the store parameter to specify the directory, the progress information is stored in the specified directory. If you use the num_threads to set the number of concurrent upload threads, ensure that the value of oss2.defaults.connection_pool_size is no less than the thread number.. The number of concurrent threads is 1 by default.
oss2.resumable_upload(bucket, '<yourObjectName>', '<yourLocalFile>',
    store=oss2. ResumableStore(root='/tmp'),
    multipart_threshold=100*1024,
    part_size=100*1024,
    num_threads=4)
```

For more information about resumable upload, see [Resumable upload](../../../../../reseller.en-US/Developer Guide/Upload files/Multipart upload.md#) in the OSS Developer Guide.

**Note:** 

-   Multiple threads are launched in resumable uploads or downloads. Therefore, you do not need to encapsulate multiple upload or download threads when calling the method. Otherwise, data may be transmitted repeatedly.
-   We recommend you set a larger part size when the network connection is stable. Otherwise, set a smaller part size.

