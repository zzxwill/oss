# Protect data through client encryption {#concept_xgh_llf_vdb .concept}

Client encryption means that the encryption is completed before the user data is sent to the remote server, whereas the plaintext of the key used for encryption is kept in the local computer only. Therefore, the security of user data can be ensured because others cannot decrypt the data to obtain the original data even if the data leaks.

This document describes how to protect data through client encryption based on the current Python SDK version of OSS.

## Principles {#section_rkz_nlf_vdb .section}

1.  The user maintains a pair of RSA keys \(`rsa_private_key` and `rsa_public_key`\) in the local computer.
2.  Each time when any object is uploaded, a symmetric key **data\_key** of AES256 type is generated randomly, and then `data_key` is used to encrypt the original content to obtain encrypt\_content.
3.  Use `rsa_public_key` to encrypt `data_key` to obtain `encrypt_data_key`, place it in the request header as the custom meta of the user, and send it together with encrypt\_content to the OSS.
4.  When Get Object is performed, encrypt\_content and `encrypt_data_key` in the custom meta of the user are obtained first.
5.  The user uses `rsa_private_key` to decrypt `encrypt_data_key` to obtain `data_key`, and then uses `data_key` to decrypt encrypt\_content to obtain the original content.

**Note:** The user’s key in this document is an asymmetric RSA key, and the AES256-CTR algorithm is used when object content is encrypted. For more information, see [PyCrypto Document](https://www.dlitz.net/software/pycrypto/api/2.6/). This document describes how to implement client encryption through the custom meta of an object. The user can select the encryption key type and encryption algorithm as required.

## Structural diagram {#section_fph_rlf_vdb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4423/1842_en-US.png)

## Preparation {#section_wzn_5lf_vdb .section}

1.  For installation and usage of the Python SDK, see [Quick Installation of Python SDK](https://www.alibabacloud.com/help/doc-detail/32026.htm).
2.  Install the PyCrypto library.

    ```
    pip install pycrypto
    ```


## Example of complete Python code {#section_zqg_ylf_vdb .section}

```
# -*- coding: utf-8 -*-
import os
import shutil
import base64
import random
import oss2
from Crypto.Cipher import PKCS1_OAEP
from Crypto.PublicKey import RSA
from Crypto.Cipher import AES
from Crypto import Random
from Crypto.Util import Counter
# aes 256, key always is 32 bytes
_AES_256_KEY_SIZE = 32
_AES_CTR_COUNTER_BITS_LEN = 8 * 16
class AESCipher:
    def __init__(self, key=None, start=None):
        self.key = key
        self.start = start
        if not self.key:
            self.key = Random.new().read(_AES_256_KEY_SIZE)
        if not self.start:
            self.start = random.randint(1, 10)
        ctr = Counter.new(_AES_CTR_COUNTER_BITS_LEN, initial_value=self.start)
        self.cipher = AES.new(self.key, AES.MODE_CTR, counter=ctr)
    def encrypt(self, raw):
        return self.cipher.encrypt(raw)
    def decrypt(self, enc):
        return self.cipher.decrypt(enc)
# First, initialize the information such as AccessKeyId, AccessKeySecret, and Endpoint.
# Obtain the information through environment variables or replace the information such as "<Your AccessKeyId>" with the real AccessKeyId, and so on.

# Use Hangzhou region as an example. Endpoint can be:
# http://oss-cn-hangzhou.aliyuncs.com
# https://oss-cn-hangzhou.aliyuncs.com
# Access using the HTTP and HTTPS protocols respectively.
access_key_id = os.getenv('OSS_TEST_ACCESS_KEY_ID', '<your AccessKeyId>')
access_key_secret = os.getenv('OSS_TEST_ACCESS_KEY_SECRET', '<Your AccessKeySecret>')
bucket_name = os.getenv('OSS_TEST_BUCKET', '<Your Bucket>')
endpoint = os.getenv('OSS_TEST_ENDPOINT', '<Your Access Domain Name>')
# Make sure that all the preceding parameters have been filled in correctly.
for param in (access_key_id, access_key_secret, bucket_name, endpoint):
    assert '<' not in param, 'Please set the parameter:' + param
##### 0 prepare ########
# 0.1 Generate the RSA key file and save it to the disk 
rsa_private_key_obj = RSA.generate(2048)
rsa_public_key_obj = rsa_private_key_obj.publickey()
encrypt_obj = PKCS1_OAEP.new(rsa_public_key_obj)
decrypt_obj = PKCS1_OAEP.new(rsa_private_key_obj)
# save to local disk 
file_out = open("private_key.pem", "w")
file_out.write(rsa_private_key_obj.exportKey())
file_out.close()
file_out = open("public_key.pem", "w")
file_out.write(rsa_public_key_obj.exportKey())
file_out.close()
# 0.2 Create the Bucket object. All the object-related interfaces can be implemented by using the Bucket object
bucket = oss2. Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
obj_name = 'test-sig-1'
content = "test content"
#### 1 Put Object ####
# 1.1 Generate the one-time symmetric key encrypt_cipher used to encrypt this object, where key and start are values generated at random
encrypt_cipher = AESCipher()
# 1.2 Use the public key to encrypt the information for assisting encryption, and save it in the custom meta of the object. When Get Object is performed later, we can use the private key to perform decryption and obtain the original content according to the custom meta
headers = {}
headers['x-oss-meta-x-oss-key'] = base64.b64encode(encrypt_obj.encrypt(encrypt_cipher.key))
headers['x-oss-meta-x-oss-start'] = base64.b64encode(encrypt_obj.encrypt(str(encrypt_cipher.start)))
# 1.3. Use encrypt_cipher to encrypt the original content to obtain encrypt_content
encryt_content = encrypt_cipher.encrypt(content)
# 1.4 Upload the object
result = bucket.put_object(obj_name, encryt_content, headers)
if result.status / 100 ! = 2:
    exit(1)
#### 2 Get Object ####
# 2.1 Download the encrypted object
result = bucket.get_object(obj_name)
if result.status / 100 ! = 2:
    exit(1)
resp = result.resp
download_encrypt_content = resp.read()
# 2.2 Resolve from the custom meta the key and start that are previously used to encrypt this object 
download_encrypt_key = base64.b64decode(resp.headers.get('x-oss-meta-x-oss-key', ''))
key = decrypt_obj.decrypt(download_encrypt_key)
download_encrypt_start = base64.b64decode(resp.headers.get('x-oss-meta-x-oss-start', ''))
start = int(decrypt_obj.decrypt(download_encrypt_start))
# 2.3 Generate the cipher used for decryption, and decrypt it to obtain the original content
decrypt_cipher = AESCipher(key, start)
download_content = decrypt_cipher.decrypt(download_encrypt_content)
if download_content ! = content:
    print "Error!"
else:
    print "Decrypt ok. Content is: %s" % download_content
```

