# Client-side encryption {#concept_74371_zh .concept}

Client-side encryption is performed locally on data before the data is uploaded to OSS.

For the complete code of client-side encryption, see [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/object_crypto.py).

You can use the following two methods to manage keys:

-   Manually manage keys \(RSA\).
-   Use Key Management Service \(KMS\) to manage keys.

Using the two methods, you can prevent the keys from being leaked and therefore ensure the security of data on the client. Additionally, others cannot decrypt the data to obtain the original data even the data is leaked.

For detailed information about client-side encryption, see [Introduction to client-side encryption SDK](https://partners-intl.aliyun.com/help/doc-detail/73332.htm) in the OSS Developer Guide.

## Encryption meta information {#section_ozb_rpp_kfb .section}

|Parameter|Description|Required or not|
|:--------|:----------|:--------------|
|x-oss-meta-oss-crypto-key|Specifies the encrypted key, which is a string encoded with Base64 after being encryted by the RSA algorithm.|Required|
|x-oss-meta-oss-crypto-start|Specifies the initial value of the encrypted data, which is generated randomly. The initial value is a string encoded with Base64 after being encryted by the RSA algorithm.|Required|
|x-oss-meta-oss-cek-alg|Specifies the encryption algorithm with the following available values: AES, GCM, and NoPadding.|Required|
|x-oss-meta-oss-wrap-alg|Specifies the encryption algorithm with the following available values: rsa and kms.|Required|
|x-oss-meta-oss-matdesc|Specifies the description of the Content Encryption Key \(CEK\) in the JSON format. This parameter does not take effect currently.|Optional|
|x-oss-meta-unencrypted-content-length|Specifies the length of data before encryption. This parameter is not generated if the content-length is not specified.|Optional|
|x-oss-meta-unencrypted-content-md5|Specifies the MD5 of the data before encryption. If the MD5 is not specified, this parameter is not generated.|Optional|

## Upload and download objects with the keys manually managed { .section}

You can upload and download objects with the keys manually managed. The code is as follows:

```language-python
# -*- coding: utf-8 -*-
import os
import oss2
from  oss2.crypto import LocalRsaProvider

# It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')

# Create a bucket and use the RSA method to encrypted the data. This method
bucket = oss2.CryptoBucket(auth,'<yourEndpoint>', '<yourBucketName>', crypto_provider=LocalRsaProvider())

key = 'motto.txt'
content = b'a' * 1024 * 1024
filename = 'download.txt'


# Upload an object.
bucket.put_object(key, content, headers={'content-length': str(1024 * 1024)})

# Download an object.
result = bucket.get_object(key)

# Perform verification.
content_got = b''
for chunk in result:
    content_got += chunk
assert content_got == content

# Download an object to a local file.
result = bucket.get_object_to_file(key, filename)

# Perform verification.
with open(filename, 'rb') as fileobj:
    assert fileobj.read() == content

os.remove(filename)

```

## Upload and download objects with the keys managed by KMS { .section}

-   Prerequisites

    1.  You have activated the Alibaba Cloud Key Management Service.
    2.  The key ID for your region has been generated.
    3.  You have created the custom authorization policies as follows: Log on to the RAM console, select **** \> **RAM** \> **Policies** \> **Custom Policy** \> **Create Authorization Policy**, and associate corresponding users with the policy.
    Refer to the following format to create a custom policy:

    ```language-json
    {
      "Version": "1",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": [
            "kms:CreateKey",
            "kms:GenerateDataKey",
            "kms:ListKeys",
            "kms:Encrypt",
            "kms:Decrypt"
          ],
          "Resource": [
            "acs:kms:<yourRegion>:<yourloginUserId>:key/*"
          ]
        }
      ]
    }
    
    ```

-   Sample code

    ```language-python
    # -*- coding: utf-8 -*-
    import os
    import oss2
    from  oss2.crypto import AliKMSProvider
    
    # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    
    # Create a bucket and use the KMS method to encrypt the data. This method only applies to scenarios where objects are uploaded or downloaded entirely.
    bucket = oss2.CryptoBucket(auth,'<yourEndpoint>', 'liusiman123456',crypto_provider=AliKMSProvider('<yourAccessKeyId>', '<yourAccessKeySecret>', '<yourRegion>', '<yourCMK>', '1234'))
    
    key = 'motto.txt'
    content = b'a' * 1024 * 1024
    filename = 'download.txt'
    
    # Upload an object.
    bucket.put_object(key, content, headers={'content-length': str(1024 * 1024)})
    
    # Download an object.
    result = bucket.get_object(key)
    
    # Perform verification.
    content_got = b''
    for chunk in result:
        content_got += chunk
    assert content_got == content
    
    # Download an object to a local file.
    result = bucket.get_object_to_file(key, filename)
    
    # Perform verification.
    with open(filename, 'rb') as fileobj:
        assert fileobj.read() == content
    
    os.remove(filename)
    
    ```


