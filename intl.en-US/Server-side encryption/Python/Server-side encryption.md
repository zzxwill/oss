# Server-side encryption {#concept_265074 .concept}

OSS supports server-side encryption for uploaded data. This means that when user data is uploaded, OSS encrypts the data and permanently stores the data. Then, when the data is downloaded by a user, OSS automatically decrypts the data, returns the original data to the user, and declares in the header of the response that the data has been encrypted on the server.

The following server-side encryption methods are available for different application scenarios:

-   Server-side encryption that uses CMKs managed by KMS for encryption and decryption \(SSE-KMS\)

    When uploading an object, you can use a specified CMK ID or the default CMK managed by KMS to encrypt and decrypt a large amount of data. This method is cost-effective because you do not need to send user data to the KMS server through networks for encryption and decryption.

    **Note:** API calling fees are incurred if you use CMKs to encrypt or decrypt data.

-   Server-side encryption fully managed by OSS \(SSE-OSS\)

    When you upload an object, OSS encrypts the object on the server side by using AES256 fully managed by OSS. In this method, OSS uses AES256 to encrypt each object with an individual key. Furthermore, the individual keys are encrypted by a CMK that is updated periodically for higher security. This method applies to encrypt or decrypt bulk data.


**Note:** 

-   Only one server-side encryption method can be used for an object at one time.
-   If you configure server-side encryption for a bucket, you can still configure the encryption method for a single object when uploading or copying it. In this case, the encryption method configured for the object takes precedence. For more information, see [PutObject](../../../../intl.en-US/API Reference/Object operations/PutObject.md#table_im4_mkw_bz).
-   For more information about server-side encryption, see [Server-side encryption](https://www.alibabacloud.com/help/doc-detail/119320.html).

## Configure server-side encryption for a bucket {#section_uri_y0t_bml .section}

You can run the following code to configure the default encryption method for a bucket. After the method is configured, all objects that are uploaded to the bucket without their encryption methods configured are encrypted in the configured default encryption method.

``` {#codeblock_alj_33e_qft}
# -*- coding: utf-8 -*-
import oss2
from oss2.models import ServerSideEncryptionRule

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Configures the encryption method for the bucket. In this example, AE256 is specified as an example.
rule = ServerSideEncryptionRule()
rule.sse_algorithm = oss2.SERVER_SIDE_ENCRYPTION_AES256
# Configures the CMK ID when the encryption method is set to KMS. To encrypt data by using a specified CMK, input the specified CMK ID. If you use the CMK managed by OSS to encrypt data, set this parameter to null. If you use the AES256 method to encrypt data, this parameter must be set to null.
rule.kms_master_keyid = ""

# Configures server-side encryption for the bucket.
result = bucket.put_bucket_encryption(rule)
# Checks the returned HTTP status code.
print('http response code:', result.status)             
```

## Obtain the server-side encryption settings of a bucket {#section_3vl_t28_29d .section}

You can run the following code to obtain the server-side encryption settings of a bucket:

``` {#codeblock_cmd_bti_eep}
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Obtains the server-side encryption settings of the bucket.
result = bucket.get_bucket_encryption()
# Prints the obtained server-side encryption settings.
print('sse_algorithm:', result.sse_algorithm)
print('kms_master_keyid:', result.kms_master_keyid) # If the bucket encryption method is set to AES256, the value of kms_master_keyid is None.
```

## Delete the server-side encryption settings of a bucket {#section_cjv_goy_8pp .section}

You can run the following code to delete the server-side encryption settings of a bucket:

``` {#codeblock_l0o_f7g_mct}
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Deletes the server-side encryption settings of the bucket.
result = bucket.delete_bucket_encryption()
# Checks the returned HTTP status code.
print('http status:', result.status)
```

