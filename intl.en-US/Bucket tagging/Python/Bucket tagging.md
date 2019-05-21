# Bucket tagging {#concept_265114 .concept}

By using the bucket tagging function, you can classify your buckets to manage them. For example, you can view the bills of buckets with specified tags or specify that only buckets with specified tags are returned in ListBucket operations.

-   Only the bucket owner and authorized RAM users can set tags for a bucket. Otherwise, the 403 Forbidden error is returned with the error code: AccessDenied.
-   You can add a maximum of 20 tags \(key-value pairs\) for a bucket.
-   The maximum size of a key is 64 bytes. The key of a tag cannot be null and cannot be prefixed with `http://`, `https://`, or `Aliyun`.
-   The maximum size of a tag value is 128 bytes. The value of a tag can be null.
-   The key and value of a tag must be UTF-8 encoded.
-   If you use PutBucketTagging to add tags for a bucket, the original tags added for the bucket are completely overwritten.

## Add a tag for a bucket {#section_asr_38j_dqu .section}

You can run the following code to add a tag for a bucket:

``` {#codeblock_bvo_avf_ojs}
# -*- coding: utf-8 -*-
import oss2
from oss2.models import Tagging, TaggingRule

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Creates a tagging rule.
rule = TaggingRule()
rule.add('key1', 'value1')
rule.add('key2', 'value2')

# Creates a tag.
tagging = Tagging(rule)
# Adds the tag to the bucket.
result = bucket.put_bucket_tagging(tagging)
# Checks the returned HTTP status code.
print('http status:', result.status)
```

## Obtain the tags added for a bucket {#section_5t2_so5_jl8 .section}

You can run the following code to obtain the tags for a bucket:

``` {#codeblock_pm3_rii_byw}
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.

auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Obtains the tagging information about for a bucket.
result = bucket.get_bucket_tagging()
# Views the obtained tagging rule.
tag_rule = result.tag_set.tagging_rule
print('tag rule:', tag_rule)
```

## List buckets with a specified tag {#section_nt0_gl3_aey .section}

Run the following code to list buckets with a specified tag:

``` {#codeblock_owg_ora_n24}
# -*- coding: utf-8 -*-
import oss2

# Creates a server object.
# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
service = oss2.Service(auth,  'http://oss-cn-hangzhou.aliyuncs.com')

# Inputs the tag-key and tag-value fields to the params parameter of the list_buckets interface.
params = {}
params['tag-key'] = '<yourTagging_key>'
params['tag-value'] = '<yourTagging_value>'

# Lists buckets with the specified tag.
result = service.list_buckets(params=params)
# Views the list results.
for bucket in result.buckets:
    print('result bucket_name:', bucket.name)
```

## Delete the tags added for a bucket {#section_pc6_9dz_682 .section}

You can run the following code to delete the tags added for a bucket:

``` {#codeblock_9v2_onq_jmd}
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Deletes the tags added for the bucket.
result = bucket.delete_bucket_tagging()
# Checks the returned HTTP status code.
print('http status:', result.status)
```

