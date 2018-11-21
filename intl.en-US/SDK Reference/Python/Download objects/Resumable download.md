# Resumable download {#concept_88444_zh .concept}

Large-sized objects may fail to be downloaded because the network is unstable or the program exits abnormally and the entire object needs to be downloaded again. However, the object may still fail to be downloaded after multiple attempts. Therefore, OSS provides resumable download.

The process of resumable download is as follows:

1.  A temporary local file is created. The file name consists of the original object name and a random suffix.
2.  Read the OSS object based on the range you specified in the Range header in the HTTP request and write the read content to the corresponding position of the temporary file.
3.  After the download is complete, rename the temporary file as the target file. If the target file already exists, it is overwritten. If the target file does not exist, a new file is created.

You can use oss2.resumable\_download to enable resumable download. It contains the following parameters:

|Parameter|Description|Required or optional|Default value|
|:--------|:----------|:-------------------|:------------|
|bucket|Specifies the name of a bucket.|Required|None|
|key|Specifies the name of an object.|Required|None|
|filename|Specifies the name of a local file. The OSS object is download to the local file.|Required|None|
|store|Specifies the local file that records the download progress information. Download progress information is stored in the file. If the download fails at a point, the next download starts from the point based on the recorded progress information.|Optional|The .py-OSS-download directory created under the HOME directory|
|multipart\_threshold|Specifies a length threshold for the object size. If the object size exceeds the threshold, resumable download is enabled.|Optional|10 MB|
|part\_size|Specifies the size of a part.|Optional|Auto-calculated|
|progress\_callback|Specifies the download progress callback function|Optional|None|
|num\_threads|Specifies the number of parts you need to download simultaneously.|Optional|1|

Run the following code for resumable download:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

oss2.resumable_download(bucket, '<yourObjectName>', '<yourLocalFile>')

```

Python SDK 2.1.0 and later support optional parameters for resumbale download, as shown in the following code:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Make sure that the value of oss2.defaults.connection_pool_size is no less than the number of the download threads, and the value of part_size is no less than the value of oss2.defaults.multiget_part_size.
oss2.resumable_download(bucket, '<yourObjectName>', '<yourLocalFile>',
  store=oss2.ResumableDownloadStore(root='/tmp'),
  multiget_threshold=20*1024*1024,
  part_size=10*1024*1024,
  num_threads=3)

```

**Note:** Do not allow multiple programs \(threads\) to download the same object to the same target file simultaneously by calling the method. Because the progress information of the threads overwrites each other, and the temporary file names specified by the threads may conflict.

