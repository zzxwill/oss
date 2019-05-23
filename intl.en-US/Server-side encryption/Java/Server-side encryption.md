# Server-side encryption {#concept_266387 .concept}

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
// This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Configures server-side encryption for the bucket.
ServerSideEncryptionByDefault applyServerSideEncryptionByDefault = new ServerSideEncryptionByDefault(SSEAlgorithm.KMS);
applyServerSideEncryptionByDefault.setKMSMasterKeyID("<yourTestKmsId>");
ServerSideEncryptionConfiguration sseConfig = new ServerSideEncryptionConfiguration();
sseConfig.setApplyServerSideEncryptionByDefault(applyServerSideEncryptionByDefault);
SetBucketEncryptionRequest request = new SetBucketEncryptionRequest("<yourBucketName>", sseConfig);
ossClient.setBucketEncryption(request);

// Closes the OSSClient instance.
ossClient.shutdown();      
```

## Obtain the server-side encryption settings of a bucket {#section_3vl_t28_29d .section}

You can run the following code to obtain the server-side encryption settings of a bucket:

``` {#codeblock_cmd_bti_eep}
// This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Obtains the server-side encryption settings of the bucket.
ServerSideEncryptionConfiguration sseConfig = ossClient.getBucketEncryption("<yourBucketName>");
System.out.println("get Algorithm: " + sseConfig.getApplyServerSideEncryptionByDefault().getSSEAlgorithm());
System.out.println("get kmsid: " + sseConfig.getApplyServerSideEncryptionByDefault().getKMSMasterKeyID());

// Closes the OSSClient instance.
ossClient.shutdown();
				
```

## Delete the server-side encryption settings of a bucket {#section_cjv_goy_8pp .section}

You can run the following code to delete the server-side encryption settings of a bucket:

``` {#codeblock_l0o_f7g_mct}
// This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Deletes the server-side encryption settings of the bucket.
ossClient.deleteBucketEncryption("<yourBucketName>");

// Closes the OSSClient instance.
ossClient.shutdown();
```

