# Simple upload {#concept_84781_zh .concept}

Simple upload includes streaming upload and file upload. Streaming upload uses InputStream as the source object. File upload uses a local file as the source object.

For the complete simple upload code, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/SimpleGetObjectSample.java).

## Streaming upload {#section_lwr_slb_kfb .section}

Use ossClient.putObject to upload a data stream to OSS.

-   Upload a string

    Run the following code to upload a string:

    ```language-java
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Upload a string.
    String content = "Hello OSS";
    ossClient.putObject("<yourBucketName>", "<yourObjectName>", new ByteArrayInputStream(content.getBytes()));
    
    // Close your OSSClient instance.
    ossClient.shutdown();
    
    ```

-   Upload a byte array

    Run the following code to upload a byte array:

    ```language-java
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId,accessKeySecret);
    
    // Upload a byte array.
    byte[] content = "Hello OSS".getBytes();
    ossClient.putObject("<yourBucketName>", "<yourObjectName>", new ByteArrayInputStream(content));
    
    // Close your OSSClient instance.
    ossClient.shutdown();
    
    ```

-   Upload a network stream

    Run the following code to upload a network stream:

    ```language-java
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Upload a network stream.
    InputStream inputStream = new URL("https://www.aliyun.com/").openStream();
    ossClient.putObject("<yourBucketName>", "<yourObjectName>", inputStream);
    
    // Close your OSSClient instance.
    ossClient.shutdown();
    
    ```

-   Upload a file stream

    Run the following code to upload a file stream:

    ```language-java
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. To ensure cloud security, we recommend that you follow best practices of Resource Access Management and log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Upload a file stream.
    InputStream inputStream = new FileInputStream("<yourlocalFile>");
    ossClient.putObject("<yourBucketName>", "<yourObjectName>", inputStream);
    
    // Close your OSSClient instance.
    ossClient.shutdown();
    
    ```


## File upload { .section}

Run the following code to upload a local file:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Upload a file. <yourLocalFile> consists of a local file path and a file name that includes an extension, for example, /users/local/myfile.txt.
ossClient.putObject("<yourBucketName>", "<yourObjectName>", new File("<yourLocalFile>"));

// Close your OSSClient instance.
ossClient.shutdown();

```

