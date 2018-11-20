# Manage Object Meta {#concept_84840_zh .concept}

Object Meta includes HTTP headers and user-defined Object Meta. For more information, see [Object Meta](../../../../reseller.en-US/Developer Guide/Manage files/Object Meta.md#) in OSS Developer Guide.

## Configure HTTP headers { .section}

-   Configure HTTP headers

    Run the following code to configure HTTP headers:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    String content = "Hello OSS";
    
    // Create Object Meta. You can configure HTTP headers with Object Meta.
    ObjectMetadata meta = new ObjectMetadata();
    
    String md5 = BinaryUtil.toBase64String(BinaryUtil.calculateMd5(content.getBytes()));
    // Start object content verification with MD5 values. After the verification is started, the MD5 value calculated by OSS is compared with that of an object. If the two values are different, an error occurs.
    meta.setContentMD5(md5);
    // Specify the type of content you want to upload. The content type determines the form and format in which your browser reads an object. The object extension is used as the content type if the content type is not specified. If no object extension exists, the default value application/octet-stream is used.
    meta.setContentType("text/plain");
    // Specify the name of the content when you download an object.
    meta.setContentDisposition("attachment; filename=\"DownloadFilename\"");
    // Configure the length for an object. If the actual object length is greater than the configured length, only the configured length will be uploaded. If the configured length is greater than the actual object length, the entire object will be uploaded.
    meta.setContentLength(content.length());
    // Configure the cache action of a webpage when an object is downloaded.
    meta.setCacheControl("Download Action");
    // Configure the expiration time (GMT is used).
    meta.setExpirationTime(DateUtil.parseIso8601Date("2022-10-12T00:00:00.000Z"));
    // Configure the encoding format when an object is downloaded.
    meta.setContentEncoding("utf-8");
    // Configure the HTTP header.
    meta.setHeader("<yourHeader>", "<yourHeaderValue>");
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Upload the object.
    ossClient.putObject("<yourBucketName>", "<yourObjectName>", new ByteArrayInputStream(content.getBytes()), meta);
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

    For more information about HTTP headers, see [RFC2616](https://tools.ietf.org/html/rfc2616).

-   Configure user-defined Object Meta

    You can define user-defined Object Meta for object descriptions.

    Run the following code to configure user-defined Object Meta:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    String content = "Hello OSS";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Create Object Meta.
    ObjectMetadata meta = new ObjectMetadata();
    
    // Set property to property-value for user-defined Object Meta. We recommend that you use Base64 encoding method.
    meta.addUserMetadata("property", "property-value");
    
    // Upload the object.
    ossClient.putObject("<yourBucketName>", "<yourObjectName>", new ByteArrayInputStream(content.getBytes()), meta);
    
    // Obtain Object Meta.
    ossClient.getObjectMetadata("<yourBucketName>", "<yourObjectName>");
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

    When an object is downloaded, its Object Meta is also downloaded. An object can contain multiple pieces of Object Meta, with the total maximum size smaller than 8 KB.


## Modify Object Meta { .section}

Run the following code to modify Object Meta:

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String sourceBucketName = "<yourSourceBucketName>";
String sourceObjectName = "<yourSourceObjectName>";
String destinationBucketName = "<yourDestinationBucketName>";
String destinationObjectName = "<yourDestinationObjectName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId,accessKeySecret);

// Replace the source object with the target object. Call ossClient.copyObject to modify Object Meta.
CopyObjectRequest request = new CopyObjectRequest(sourceBucketName, sourceObjectName, destinationBucketName, destinationObjectName);

ObjectMetadata meta = new ObjectMetadata();
// Specify the type of content you want to upload. The content type determines the form and format that your browser uses to read an object. The object extension is used for the content type if the content type is not specified. If no object extension exists, the default value application/octet-stream is used.
meta.setContentType("text/plain");
// Specify the name of the content when you download an object.
meta.setContentDisposition("Download File Name");
// Configure the cache action of a webpage when an object is downloaded.
meta.setCacheControl("Download Action");
// Configure the expiration time (GMT is used).
meta.setExpirationTime(DateUtil.parseIso8601Date("2022-10-12T00:00:00.000Z"));
// Configure the encoding type when the object is downloaded.
meta.setContentEncoding("utf-8");
// Configure the HTTP header.
meta.setHeader("<yourHeader>", "<yourHeaderValue>");
// Set property to property-value for user-defined Object Meta.
meta.addUserMetadata("property", "property-value");
request.setNewObjectMetadata(meta);

//Modify Object Meta.
ossClient.copyObject(request);

// Close your OSSClient.
ossClient.shutdown();

```

## Obtain Object Meta { .section}

The following table lists the methods you can use to obtain Object Meta.

|Method|Description|Advantage|
|:-----|:----------|:--------|
|ossClient.getSimplifiedObjectMeta|Obtains ETag, Size, and LastModified \(the latest time an object is modified.\) for the object.|More lightweight and faster|
|ossClient.getObjectMetadata|Obtains all metadata.|None|

Run the following code to obtain Object Meta:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Obtain partial Object Meta.
SimplifiedObjectMeta objectMeta = ossClient.getSimplifiedObjectMeta("<yourBucketName>", "<yourObjectName>");
System.out.println(objectMeta.getSize());
System.out.println(objectMeta.getETag());
System.out.println(objectMeta.getLastModified());

// Obtain all Object Meta.
ObjectMetadata metadata = ossClient.getObjectMetadata("<yourBucketName>", "<yourObjectName>");
System.out.println(metadata.getContentType());
System.out.println(metadata.getLastModified());
System.out.println(metadata.getExpirationTime());

// Close your OSSClient.
ossClient.shutdown();

```

