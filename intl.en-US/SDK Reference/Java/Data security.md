# Data security {#concept_85170_zh .concept}

OSS Java SDK provides data integrity checks and file encryption on the server and client to guarantee data security during file \(object\) uploads, downloads, and replication.

## Data integrity checks {#section_wll_pxc_kfb .section}

OSS Java SDK provides MD5 authentication and cyclic redundancy checks \(CRC\) to guarantee data integrity for object \(file\) uploads, downloads, and replication.

-   MD5 authentication

    If you configure Content-MD5 for file uploads, OSS calculates the MD5 value based on the received content. If the MD5 value is inconsistent with that of the uploaded file, InvalidDigest is returned. Thus, data integrity is guaranteed.

    Run the following code for MD5 authentication during the file upload:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Upload a string.
    String content = "Hello OSS";
    
    ObjectMetadata meta = new ObjectMetadata();
    // Configure MD5 authentication.
    String md5 = BinaryUtil.toBase64String(BinaryUtil.calculateMd5(content.getBytes()));
    meta.setContentMD5(md5);
    
    ossClient.putObject("<yourBucketName>", "<yourObjectName>", new ByteArrayInputStream(content.getBytes()), meta);
    
    // Close your OSSClient instance.
    ossClient.shutdown();
    
    ```

    **Note:** MD5 authentication can be used for putObject, getObject, appendObject, postObject, and uploadPart.

-   CRC64

    CRC is enabled by default during object \(file\) uploads, downloads, and replication to guarantee data integrity.

    Run the following code to check whether CRC takes effect during the file upload:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Upload a string.
    String content = "Hello OSS";
    
    PutObjectResult result = ossClient.putObject("<yourBucketName>", "<yourObjectName>", new ByteArrayInputStream(content.getBytes()));
    
    // Check whether CRC is enabled. True indicates it is enabled while false indicates it is disabled.
    if (ossClient.getClientConfiguration().isCrcCheckEnabled()) {
    	// Check whether CRC takes effect based on the response.
        OSSUtils.checkChecksum(result.getClientCRC(), result.getServerCRC(), result.getRequestId());
     }
    // Close your OSSClient instance.
    ossClient.shutdown();
    
    ```

    **Note:** 

    -   You can use CRC64 authentication for putObject, getObject, appendObject, and uploadPart.
    -   CRC64 authentication affects the upload and download progress because CRC64 uses CPU space.

## File encryption on the server { .section}

You can set the serverSideEncryption parameter in Object Meta to AES256 or KMS for file uploads to encrypt the file and save it on the server. For more information, see [Server-side encryption](../../../../reseller.en-US/Developer Guide/Security management/Server-side encryption.md#) in the OSS Developer Guide.

Run the following code for file encryption on the server during file uploads:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Upload a string.
String content = "Hello OSS";

ObjectMetadata meta = new ObjectMetadata();
// Set the server-side encryption code to AES256 for the file.
meta.setServerSideEncryption(ObjectMetadata.AES_256_SERVER_SIDE_ENCRYPTION);

ossClient.putObject("<yourBucketName>", "<yourObjectName>", new ByteArrayInputStream(content.getBytes()), meta);

// Close your OSSClient instance.
ossClient.shutdown();

```

## Client-side encryption { .section}

To encrypt a file on the client, you can encrypt the data on your local device, upload it to OSS, download it to the local device, and decrypt it. For more information, see [Introduction to client-side encryption SDK](https://partners-intl.aliyun.com/help/doc-detail/73332.htm) in OSS Developer Guide.

