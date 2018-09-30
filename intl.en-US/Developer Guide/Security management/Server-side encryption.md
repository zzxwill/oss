# Server-side encryption {#concept_lqm_fkd_5db .concept}

This article describes how to use the server-side encryption feature of OSS to encrypt and protect the persistent data in OSS:

-    [Server-side encryption options](#section_jhs_zld_5db)
-    [Fully managed by OSS](#section_x5x_cmd_5db)
-    [CMK managed by KMS](#section_awd_2md_5db)
-   [Server-side encryption API usage](#section_mbf_hmd_5db)

OSS supports server-side encryption for the data uploaded by users: When a user uploads data, OSS encrypts the user data and permanently stores the data with encryption; when the user downloads the data, OSS automatically decrypts the encrypted data, returns the original data to the user, and declares in the header of the returned HTTP request that the data has been encrypted on the server.

## Main application scenarios of OSS server-side encryption {#section_gvf_xld_5db .section}

OSS protects static data by using server-side encryption, which applies to scenarios with high security or compatibility requirements for storage, such as the storage of deep learning sample files and online collaboration document data. The following server-side encryption methods are available for different application scenarios:

-   Using KMS for encryption and decryption

    When uploading a file, you can use a specified CMK ID or the CMK fully managed by KMS to encrypt and decrypt a large amount of data. You do not need to send user data to the KMS service side through networks for encryption and decryption, and therefore is a low-cost method.

-   Using encryption fully managed by OSS

    Encryption fully managed by OSS is a property of objects. When creating an object, you only need to add the HTTP header, x-oss-server-side-encryption, to the Put Object request and specify its value as AES256 to encrypt and store the object on the server. This method is ideal for bulk data 's encryption and decryption.


## Server-side encryption options {#section_jhs_zld_5db .section}

OSS server-side encryption provides the following options for users to choose from \(based on the key management policy\):

-   Fully managed by OSS: The generation and management of data encryption keys are conducted by OSS, which provides strong and multi-factor security measures to protect data. The data encryption algorithm adopted is the strong encryption algorithm of AES-256 \(256-bit advanced encryption standard\).

## Server-side encryption - Fully managed by OSS {#section_x5x_cmd_5db .section}

The OSS server-side encryption is a property of objects. When creating an object, the user only needs to add the HTTP header, `x-oss-server-side-encryption`, to the Put Object request and specify its value as `AES256` to encrypt and store the object on the server.

The following operations support the addition of the HTTP header to requests:

-   Put Object: simple upload
-   Copy Object: copy an object
-   Initiate Multipart Upload: multipart upload

## Server-side encryption - CMK managed by KMS {#section_awd_2md_5db .section}

When creating an object, the user only needs to add the HTTP header, `x-oss-server-side-encryption` to the request and specify its value as`KMS`, to encrypt and store the object on the server \(KMS service is used for key management\). With the KMS service, OSS can encrypt and protect the objects stored in OSS by using strong security measures such as envelope encryption and AES-256 encryption algorithm.

KMS is a secure and easy-to-use management service provided by Alibaba Cloud. With the help of KMS, users no longer have to spend a great deal to protect the confidentiality, integrity, and availability of keys. Users can use keys securely and conveniently, and focus on developing encryption/decryption function scenarios. Users can view and manage KMS keys in the KMS console.

The following operations support adding the HTTP header to requests

-   Put Object: simple upload
-   Copy Object: copy an object
-   Initiate Multipart Upload: multipart upload

**Note:** 

-   Users must activate the KMS service. In this mode, when a user adds objects for encryption to a bucket in a region for the first time, a default KMS key is automatically created in this region \(used as CMK\), which is used for server-side encryption \(KMS mode\).
-   Users can view the information about this key in the KMS console. See [KMS User Guide](https://partners-intl.aliyun.com/help/doc-detail/28943.htm).

## Server-side encryption - RestAPI usage {#section_mbf_hmd_5db .section}

**Operation requests**

The following APIs support adding the header `x-oss-server-side-encryption` to requests:

-   Put Object: simple upload
-   Copy Object: copy an object
-   Initiate Multipart Upload: multipart upload

**HTTP header format**

|Name|Description|Example|
|:---|:----------|:------|
|x-oss-server-side-encryption|Note: This option specifies the server-side encryption mode  Valid values: `AES256` ,`KMS`|`x-oss-server-side-encryption:KMS` indicates the server-side encryption mode is CMK managed by KMS|

**Note:** 

-   If any other request received by OSS contains the header `x-oss-server-side-encryption`, OSS directly returns HTTP Status Code 400, with the error code in the message body being InvalidArgument.
-   If a user specifies an invalid value for the header `x-oss-server-side-encryption`, OSS directly returns HTTP Status Code 400, with the error code in the message body being `InvalidEncryptionAlgorithmError`.

**Operation response**

For objects stored after server-side encryption, OSS returns the header `x-oss-server-side-encryption` for the following API requests:

-   Put Object
-   Copy Object
-   Initiate Multipart Upload
-   Upload Part
-   Complete Multipart Upload
-   Get Object
-   Head Object

**Meta information**

For objects stored in the mode of CMK managed by KMS, their Meta information includes the following fields:

|Name|Description|Example|
|:---|:----------|:------|
|x-oss-server-side-encryption|Specifies the server-side encryption mode|`x-oss-server-side-encryption: KMS`|
|x-oss-server-side-encryption-key-id|The ID of the user KMS key used for encrypting the object|`x-oss-server-side-encryption-key-id: 72779642-7d88-4a0f-8d1f-1081a9cc7afb`|

**Related APIs**

-   [AppendObject](../../../../reseller.en-US/API Reference/Object operations/AppendObject.md#)
-   [PutObject](../../../../reseller.en-US/API Reference/Object operations/PutObject.md#)
-   [CopyObject](../../../../reseller.en-US/API Reference/Object operations/CopyObject.md#)
-   [PostObject](../../../../reseller.en-US/API Reference/Object operations/PostObject.md#)

