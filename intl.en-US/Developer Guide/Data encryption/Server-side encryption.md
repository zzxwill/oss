# Server-side encryption {#concept_lqm_fkd_5db .concept}

This topic describes how to use the server-side encryption feature provided by OSS to encrypt and protect persistent data stored in OSS.

OSS supports server-side encryption for uploaded data. This means that when user data is uploaded, OSS encrypts the data and permanently stores the data. Then, when the data is downloaded by a user, OSS automatically decrypts the data, returns the original data to the user, and declares in the header of the returned HTTP request that the data has been encrypted on the server.

## Scenarios {#section_gvf_xld_5db .section}

The following server-side encryption methods are available for different application scenarios:

-   Server-side encryption that uses CMKs managed by KMS for encryption and decryption \(SSE-KMS\)

    When uploading an object, you can use a specified CMK ID or the default CMK managed by KMS to encrypt and decrypt a large amount of data. This method is cost-effective because you do not need to send user data to the KMS service side through networks for encryption and decryption.

    **Note:** 

-   Server-side encryption fully managed by OSS \(SSE-OSS\)

    This encryption method is a property of an object. When sending a request to upload an object or modify the metadata of an object, you can include the `X-OSS-server-side-encrpytion` header in the request and specify its value as AES256. In this method, OSS uses AES256 to encrypt each object with an individual key. Furthermore, the individual keys are encrypted by a customer master key \(CMK\) that is updated periodically for higher security. This method applies to encrypt or decrypt bulk data.


**Note:** Only one server-side encryption method can be used for an object at one time.

## Configuration {#section_lr4_cbm_vgb .section}

For detailed information about server-side encryption configuration, see [Protect data by performing server-side encryption](../../../../../reseller.en-US/Best Practices/Data security/Protect data by performing server-side encryption.md#).

## Server-side encryption that uses CMKs managed by KMS for encryption and decryption {#section_c24_wbd_5gb .section}

Key Management Service \(KMS\) is a secure and easy-to-use key protection and management service provided by Alibaba Cloud. KMS allows you to use keys in a secure manner, at minimal cost. You can view and manage keys in the KMS console.

To encrypt an object when creating it, you can include the `x-oss-server-side-encryption` header in the request and specify its value to `KMS` \(which indicates that KMS is used for key management\).

In addition to the usage of AES256 encryption algorithm, KMS stores the customer master key \(CMK\) used to encrypt data keys, generate data keys, and uses the envelope encryption mechanism to protect data from unauthorized access.

The following table shows the logic of SSE-KMS.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4384/155134082038833_en-US.png)

A CMK can be generated in the following methods:

-   Use the default CMK managed by KMS.

    When sending a request to upload an object or modify the metadata of an object, you can include the `X-OSS-server-side-encrpytion` header in the request and specify its value as KMS without a specified CMK ID. In this method, OSS generates an individual key to encrypt each object by using the default managed CMK, and automatically decrypts the object when it is downloaded.

-   Use a CMK specified by the user.

    When sending a request to upload an object or modify the metadata of an object, you can include the `X-OSS-server-side-encrpytion` header in the request, specify its value as KMS, and specify the value of `X-oss-server-side-encrpytion-key-id` to a specified CMK ID. In this method. OSS generates an individual key to encrypt each object by using the specified CMK, and adds the CMK ID used to encrypt an object into the metadata of the object so that the object is automatically decrypted when it is downloaded by an authorized user.

-   Use the BYOK material of the user as the CMK.

    You can import your BYOK material into KMS as the CMK as follows:

    1.  Create a CMK without key material.
    2.  Import the key material from an external source.
    For more information about how to import key material, see [Import key material](../../../../../reseller.en-US/User Guide/Import key material.md#).


**Note:** 

-   If you use a CMK to encrypt an object, the data key used in the encryption is also encrypted and is stored as the metadata of the object.
-   In server-side encryption that uses the default CMK managed by KMS, only the data in the object is encrypted. The metadata of the object is not encrypted.
-   To use a RAM user to encrypt objects with a specified CMK, you must grant the relevant permissions to the RAM user. For more information, see [Use RAM for KMS resource authorization](../../../../../reseller.en-US/User Guide/Use RAM for KMS resource authorization.md#).

## Server-side encryption fully managed by OSS {#section_x5x_cmd_5db .section}

In this server-side encryption method, OSS generates and manages the keys used for data encryption, and provides strong multi-factor security measures to protect data. AES256 \(256-bit advanced encryption standard\), a strong encryption algorithm, is used to encrypt data.

In this way, the encryption method becomes a property of an object. To perform server-side encryption on an object, you can include the `X-OSS-server-side-encrpytion` header in the PutObject request and specify its value as `AES256`.

## APIs that support server-side encryption {#section_mbf_hmd_5db .section}

**Note:** This function is in the beta testing phase. To join the testing group, contact Alibaba Cloud technical support or open a ticket.

-   APIs that support server-side encryption in requests

    The `x-oss-server-side-encryption` header is supported in requests initiated by the following APIs:

    -   PutObject
    -   CopyObject
    -   InitiateMultipartUpload
    The following table describes the HTTP headers that can be included in requests.

    |Header|Description|Example|
    |:-----|:----------|:------|
    |x-oss-server-side-encryption|Specifies the server-side encryption method.Valid values: AES256 and KMS

|`x-oss-server-side-encryption:KMS` indicates that the server-side encryption uses CMKs managed by KMS.|
    |x-oss-server-side-encryption-key-id|Specifies the ID of the CMK used to encrypt the object.This header must be specified when you use a specified CMK ID for encryption.

|`x-oss-server-side-encryption-key-id: 72779642-7d88-4a0f-8d1f-1081a9cc7afb`|

    **Note:** 

    -   If the `x-oss-server-side-encryption` header is included in requests initiated by APIs except for PutObject, CopyObject, and InitiateMultipartUpload, OSS returns HTTP status code 400 and includes `InvalidArgument` in the error message.
    -   If an invalid value is specified for the `x-oss-server-side-encryption` header, OSS returns HTTP status code 400 and includes `InvalidEncryptionAlgorithmError` in the error message.
-   APIs that support server-side encryption in responses

    OSS includes the `x-oss-server-side-encryption` header in responses to requests initiated by the following APIs to access objects encrypted at the server side.

    -   PutObject
    -   CopyObject
    -   InitiateMultipartUpload
    -   UploadPart
    -   CompleteMultipartUpload
    -   GetObject
    -   HeadObject

