# Bucket {#concept_32029_zh .concept}

A bucket serves as a container that stores objects. Objects belong to a bucket.

## Create a bucket {#section_rvh_l1j_kfb .section}

Run the following code to create a bucket:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# You can create a new bucket in a specified region by specifying the endpoint and bucket name. This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
 
# The default storage class and ACL of the created bucket are set to standard and private read respectively.
bucket.create_bucket()

```

For more information about bucket naming rules, see [naming conventions](../../../../reseller.en-US/Developer Guide/Basic concepts.md#section_yxy_jmt_tdb) in [Basic concepts](../../../../reseller.en-US/Developer Guide/Basic concepts.md#).

You can specify the ACL and the storage class when you create a bucket, as shown in [Bucket-level permissions](../../../../reseller.en-US/Developer Guide/Manage buckets/Set bucket read and write permissions.md#) and [Storage class](../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#). The sample code is as follows:

```language-python
# Set the bucket ACL to public read and set the storage class to Infrequent Access.
bucket.create_bucket(oss2. BUCKET_ACL_PUBLIC_READ, oss2.models.BucketCreateConfig(oss2. BUCKET_STORAGE_CLASS_IA))

```

## List buckets { .section}

Run the following code to list buckets:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
service = oss2. Service(auth, 'http://oss-cn-hangzhou.aliyuncs.com')

print([b.name for b in oss2. BucketIterator(service)])

```

## Configure an ACL for a bucket { .section}

The ACL of a bucket includes the following permissions.

|Permission|Description|Value|
|:---------|:----------|:----|
|Private|The bucket owner and the authorized users can read and write objects in the bucket. Other users cannot perform any operation on the objects.|oss2. BUCKET\_ACL\_PRIVATE|
|Public read|The bucket owner and the authorized users can read and write objects in the bucket. Other users can only read the objects in the bucket. Authorize this permission with caution.|oss2. BUCKET\_ACL\_PUBLIC\_READ|
|Public read-write|All users can read and write objects in the bucket. Authorize this permission with caution.|oss2. BUCKET\_ACL\_PUBLIC\_READ\_WRITE|

Run the following code to configure an ACL for a bucket:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Set the ACL for the bucket to private.
bucket.put_bucket_acl(oss2. BUCKET_ACL_PRIVATE)

```

## Obtain the ACL for a bucket { .section}

Run the following code to obtain the ACL for a bucket:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

print(bucket.get_bucket_acl().acl)

```

## Obtain bucket information { .section}

Run the following code to obtain bucket information:

```language-python
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Obtain bucket information
bucket_info = bucket.get_bucket_info()
print('name: ' + bucket_info.name)
print('storage class: ' + bucket_info.storage_class)
print('creation date: ' + bucket_info.creation_date)
print('intranet_endpoint: ' + bucket_info.intranet_endpoint)
print('extranet_endpoint ' + bucket_info.extranet_endpoint)
print('owner: ' + bucket_info.owner.id)
print('grant: ' + bucket_info.acl.grant)

```

## Delete a bucket { .section}

Before a bucket is deleted, ensure that all objects in the bucket and fragments that are generated from multipart upload are deleted.

**Note:** To delete the fragments that are generated from multipart upload, use [Bucket.ListMultipartUploads](../../../../reseller.en-US/API Reference/Multipart upload operations/ListMultipartUploads.md#) to list all fragments, and then use [Bucket.AbortMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/AbortMultipartUpload.md#) to delete the fragments.

Run the following code to delete a bucket:

```
# -*- coding: utf-8 -*-
import oss2
# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
bucket = oss2. Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
try:
    # Delete the bucket.
    bucket.delete_bucket()
except oss2.exceptions.BucketNotEmpty:
    print('bucket is not empty.')
except oss2.exceptions.NoSuchBucket:
    print('bucket does not exist')
```

For buckets which are not empty, you can list and delete objects stored in these buckets first \(stop uploading for objects uploaded by multipart upload\), and then delete the buckets.

