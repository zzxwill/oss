# Authorized access {#concept_32016_zh .concept}

This topic describes how to authorize access to users.

## Use STS for temporary access authorization {#section_nyk_bzc_kfb .section}

OSS supports Alibaba Cloud Security Token Service \(STS\) for temporary access authorization. STS is a web service that provides a temporary access token to a cloud computing user. Through the STS, you can assign a third-party application or a RAM user \(you can manage the user ID\) an access credential with a custom validity period and permissions. For more information about STS, see [STS introduction](../../../../reseller.en-US/API reference/API reference (STS)/Introduction.md#).

STS advantages:

-   Your long-term key \(AccessKey\) is not exposed to a third-party application. You only need to generate an access token and send the access token to the third-party application. You can customize access permissions and the validity of this token.
-   You do not need to keep track of permission revocation issues. The access token automatically becomes invalid when it expires.

For more information about the process of access to OSS with STS, see [RAM and STS scenario practices](../../../../reseller.en-US/Developer Guide/Access and control/Access control.md#) in OSS Developer Guide.

Run the following code to create a signature request with STS:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String securityToken = "<yourSecurityToken>";

// After a user obtains a temporary STS credential, the OSSClient is generated with the security token and temporary access key (AccessKeyId and AccessKeySecret).
// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret, securityToken);

// Perform operations on OSS.

// Close your OSSClient.
ossClient.shutdown();

```

## Sign a URL to authorize temporary access { .section}

-   Sign a URL

    You can provide a signed URL to a visitor for temporary access. When you sign a URL, you can specify the expiration time for a URL to restrict the period of access from visitors.

-   Sign a URL for access with HTTP GET

    Use the following code to sign a URL that allows access with HTTP GET:

    ```language-java
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Set the expiration time of a URL to one hour.
    Date expiration = new Date(new Date().getTime() + 3600 * 1000);
    // Generate the URL that allows access with HTTP GET. Visitors can use a browser to access relevant content.
    URL url = ossClient.generatePresignedUrl(bucketName, objectName, expiration);
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

-   Sign a URL for access with other HTTP methods

    A URL needs to be signed for temporary access from a visitor to perform other operations such as file upload and deletion. For example:

    ```language-java
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String securityToken = "<yourSecurityToken>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // After a user obtains a temporary STS credential, the OSSClient is generated with the security token and temporary access key (AccessKeyId and AccessKeySecret).
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret, securityToken);
    
    GeneratePresignedUrlRequest request = new GeneratePresignedUrlRequest(bucketName, objectName, HttpMethod.PUT);
    // Set the expiration time of a URL to one hour.
    Date expiration = new Date(new Date().getTime() + 3600 * 1000);
    request.setExpiration(expiration);
    // Set ContentType.
    request.setContentType(DEFAULT_OBJECT_CONTENT_TYPE);
    // Configure custom Object Meta.
    request.addUserMetadata("author", "aliy");
    
    // Sign a URL that allows access with HTTP PUT.
    URL signedUrl = ossClient.generatePresignedUrl(request);
    
    Map<String, String> requestHeaders = new HashMap<String, String>();
    requestHeaders.put(HttpHeaders.CONTENT_TYPE, DEFAULT_OBJECT_CONTENT_TYPE);
    requestHeaders.put(OSS_USER_METADATA_PREFIX + "author", "aliy");
    
    // Upload a file with the signed URL.
    ossClient.putObject(signedUrl, new ByteArrayInputStream("Hello OSS".getBytes()), -1, requestHeaders, true);
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

    Visitors can use a signed URL to upload a file by passing in the HttpMethod.PUT parameter.

-   Add specified parameters to a URL

    Run the following code to add specified parameters to a URL:

    ```language-java
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient  = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Create a request.
    GeneratePresignedUrlRequest generatePresignedUrlRequest = new GeneratePresignedUrlRequest(bucketName, objectName);
    // Set HttpMethod to PUT.
    generatePresignedUrlRequest.setMethod(HttpMethod.PUT);
    // Add custom Object Meta.
    generatePresignedUrlRequest.addUserMetadata("author", "baymax");
    // Add Content-Type.
    generatePresignedUrlRequest.setContentType("application/octet-stream");
    // Set the expiration time of a URL to one hour.
    Date expiration = new Date(new Date().getTime() + 3600 * 1000);
    generatePresignedUrlRequest.setExpiration(expiration);
    // Generate the signed URL.
    URL url = ossClient.generatePresignedUrl(generatePresignedUrlRequest);
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

-   Use a signed URL to obtain or upload an object
    -   Use a signed URL to obtain an object

        Run the following code to obtain a specified object through a signed URL:

        ```language-java
        // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        String objectName = "<yourObjectName>";
        
        // Create an OSSClient instance.
        OSSClient ossClient  = new OSSClient(endpoint, accessKeyId, accessKeySecret);
        
        Date expiration = DateUtil.parseRfc822Date("Wed, 18 Mar 2022 14:20:00 GMT");
        GeneratePresignedUrlRequest request = new GeneratePresignedUrlRequest(bucketName, objectName, HttpMethod.GET);
        // Configure the expiration time.
        request.setExpiration(expiration);
        // Generate a signed URL that allows HTTP GET access.
        URL signedUrl = ossClient .generatePresignedUrl(request);
        System.out.println("signed url for getObject: " + signedUrl);
        
        // Use the signed URL to send a request.
        Map<String, String> customHeaders = new HashMap<String, String>();
        // Add a request header to GetObject.
        customHeaders.put("Range", "bytes=100-1000");
        OSSObject object = ossClient.getObject(signedUrl,customHeaders);
        
        // Close your OSSClient.
        ossClient.shutdown();
        
        ```

    -   Use a signed URL to upload a file

        Run the following code to upload a file with a signed URL:

        ```language-java
        // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        String objectName = "<yourObjectName>";
        
        // Create an OSSClient instance.
        OSSClient ossClient  = new OSSClient(endpoint, accessKeyId, accessKeySecret);
        
        // Generate the signed URL.
        Date expiration = DateUtil.parseRfc822Date("Thu, 19 Mar 2019 18:00:00 GMT");
        GeneratePresignedUrlRequest request = new GeneratePresignedUrlRequest(bucketName, objectName, HttpMethod.PUT);
        // Configure the expiration time.
        request.setExpiration(expiration);
        // Configure Content-Type.
        request.setContentType("application/octet-stream");
        // Configure custom Object Meta.
        request.addUserMetadata("author", "aliy");
        // Generate a signed URL that allows access with HTTP PUT.
        URL signedUrl = ossClient.generatePresignedUrl(request);
        System.out.println("signed url for putObject: " + signedUrl);
        
        // Use the signed URL to send a request.
        File f = new File("<yourLocalFile>");
        FileInputStream fin = new FileInputStream(f);
        // Add a request header to PutObject.
        Map<String, String> customHeaders = new HashMap<String, String>();
        customHeaders.put("Content-Type", "application/octet-stream");
        customHeaders.put("x-oss-meta-author", "aliy");
        
        PutObjectResult result = ossClient.putObject(signedUrl, fin, f.length(), customHeaders);
        
        // Close your OSSClient.
        ossClient.shutdown();
        
        ```


