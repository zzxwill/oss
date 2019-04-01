# Initialization {#concept_32044_zh .concept}

The OSSClient is the Android client of OSS. It provides the caller with a series of methods to operate and manage the buckets and objects. Before you use the OSS Android SDK to initiate a request to OSS, you must first initialize an OSSClient instance and complete necessary settings.

**Note:** Make sure that the life cycle of the OSSClient is consistent with that of the application. Create a global OSSClient when an application starts, and destroy it when the application ends.

## Determine an endpoint {#section_a41_xff_lfb .section}

An endpoint is the address of Alibaba Cloud OSS in a region. It supports the following two formats:

|Endpoint type|Description|
|:------------|:----------|
|OSS region address|Address of the region where an OSS bucket is located. For more information about the endpoints in various regions, see [here](../../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).|
|User-defined domain name|Domain name defined by the user, with the CNAME directing to the OSS domain|

For more information about endpoints, [click to view details](../../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).

-   OSS region address

    You can use one of the following two methods to search for an endpoint mapped to the address of the region where an OSS bucket is located:

    -   You can query the mapping relationship between the endpoint and the region. For more information, [click to view details](../../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).
    -   You can log on to [Alibaba Cloud OSS Console](https://account.alibabacloud.com/login/login.htm), open the Bucket Overview page, and find the suffix of the bucket domain. For example, the suffix `oss-cn-hangzhou.aliyuncs.com` of the bucket domain `bucket-1.oss-cn-hangzhou.aliyuncs.com` is the endpoint of the bucket on the Internet.
-   CNAME

    You can bind your domain to a bucket through the CNAME and access the objects in the bucket through the domain.

    For example, you want to bind the domain new-image.xxxxx.com to a bucket named “image” in the Shenzhen region: you must contact your domain hosting provider for xxxxx.com to configure a new domain name resolution record used to resolve `http://new-image.xxxxx.com` to `http://image.oss-cn-shenzhen.aliyuncs.com`. The record type is CNAME.


-   Configure the endpoint and credential

    A mobile client is an untrusted environment. An extremely high risk occurs if you store the `AccessKeyId` and `AccessKeySecret` in a mobile client for signing requests. We recommend you use STS Authentication Mode or Self-signed Mode. For more information, see [Resource Access Management](intl.en-US/SDK Reference/Android/Authorized access.md#) and [Direct Transfer on a Mobile client](../../../../../intl.en-US/Best Practices/Application server/Set up direct data transfer for mobile apps.md#).

    **Note:** If you are using STS authentication mode, we recommend you use OSSAuthCredentialsProvider to access the application server. Expired tokens are updated automatically.

    For more information, [click to view details](https://github.com/aliyun/aliyun-oss-android-sdk/tree/master/app/src/main/java/com/alibaba/sdk/android/oss/app).

    An example of EndPoint and CredentialProvider settings is shown as follows:

    ```language-java
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    
    String stsServer = "STS application server address, such as http://abc.com"
    //We recommend you use OSSAuthCredentialsProvider because expired tokens can be updated quickly.
    OSSCredentialProvider credentialProvider = new OSSAuthCredentialsProvider(stsServer);
    
    //If this configuration class is not set, default configuration applies. For more information, see the description of the class.
    ClientConfiguration conf = new ClientConfiguration();
    conf.setConnectionTimeout(15 * 1000); // Connection time-out. The default value is 15 seconds.
    conf.setSocketTimeout(15 * 1000); // Socket time-out. The default value is 15 seconds.
    conf.setMaxConcurrentRequest(5); // The maximum number of concurrent requests. The default value is 5.
    conf.setMaxErrorRetry(2); // The maximum number of retries after a failure. The default value is 2.
    
    OSS oss = new OSSClient(getApplicationContext(), endpoint, credentialProvider);
    
    ```

    **Note:** For more authentication modes, see [Resource Access Management](intl.en-US/SDK Reference/Android/Authorized access.md#).

-   Configure the endpoint according to the CNAME

    If you have bound the CNAME to the bucket, you can direct the endpoint to the CNAME directly. For example,

    ```language-java
    String endpoint = "http://new-image.xxxxx.com";
    ...
    OSS oss = new OSSClient(getApplicationContext(), endpoint, credentialProvider);
    
    ```

-   Enable logging

    Mobile terminals have complicated environments during usage. The OSS SDK may fail to function in a region or during a period of time. To help developers locate issues, the OSS SDK records some logs to a local directory after the log feature is enabled. To enable the log feature, you must initialize the OSSClient before using it, by calling the following method.

    **Note:** 

    -   Log files are stored on the SD card at \\OSSLog\\logs.csv.
    -   You can upload the file to the server to further trace the issue.
    -   You can also upload the log files after connecting to Alibaba Cloud Log Service. Go to [Log Service](https://www.alibabacloud.com/product/log-service).
    ```language-objc
    //The log style.
    //You can call the OSSLog.enableLog() method to enable displaying logs in the console.
    //This setting also supports saving one copy of the log file to the SD card of the mobile phone at \OSSLog\logs.csv. This setting is not enabled by default.
    //The log records the request data, returned data and exceptions of OSS operations.
    //Such as requestId and response header. The following is a log record case.
    //android_version: 5.1. The Android version
    //mobile_model: XT1085 . The Android phone model
    //network_state: connected. The network status
    //network_type: WIFI. The network connection type
    //Specific operation information:
    //[2017-09-05 16:54:52] - Encounter local exception: //java.lang.IllegalArgumentException: The bucket name is invalid. 
    //A bucket name must: 
    //1) be comprised of lower-case characters, numbers or dash(-); 
    //2) start with lower case or numbers; 
    //3) be between 3-63 characters long. 
    //------>end of log
    OSSLog.enableLog(); //Call this method to enable logs.
    
    ```

-   Set network parameters

    You can set detailed ClientConfiguration during the initialization process as follows:

    ```language-java
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    
    // We recommend you initialize OSSClient using STS on the mobile client. For more information, see: [Resource Access Management].
    OSSCredentialProvider credentialProvider = new OSSStsTokenCredentialProvider("<StsToken.AccessKeyId>", "<StsToken.SecretKeyId>", "<StsToken.SecurityToken>");
    
    
    ClientConfiguration conf = new ClientConfiguration();
    conf.setConnectionTimeout(15 * 1000); // Connection time-out. The default value is 15 seconds.
    conf.setSocketTimeout(15 * 1000); // Socket time-out. The default value is 15 seconds.
    conf.setMaxConcurrentTaskNum(5); // The maximum number of concurrent requests. The default value is 5.
    conf.setMaxErrorRetry(2); // The maximum number of retries after a failure. The default value is 2.
    
    OSS oss = new OSSClient(getApplicationContext(), endpoint, credentialProvider, conf);
    
    ```

-   Synchronous and asynchronous interfaces in the SDK

    Network requests are not allowed to be processed in the UI thread in mobile development, so most SDK interfaces support synchronous and asynchronous calls to handle requests. After it has been called, the synchronous interface blocks other requests while waiting for the returned results. The asynchronous interface has to import a callback function that processes the result of requests.

    Synchronous interfaces cannot be called on the UI thread. If a ClientException or ServiceException occurs, it is thrown immediately. ClientException refers to local exceptions, including network exceptions, invalid parameters, and so on. ServiceException refers to service exceptions returned by OSS, such as authentication failures and server errors, among others.

    If an exception occurs while an asynchronous request is being processed, it is handled by a callback function.

    When an asynchronous interface is called, the function returns a task. You can cancel a task, wait for it to finish, or get the results directly. For example:

    ```language-java
    OSSAsyncTask task = oss.asyncGetObejct(...);
    task.cancel(); // Cancel the task
    task.waitUntilFinished(); // Wait until the task is finished
    GetObjectResult result = task.getResult(); // Block other requests and wait for the returned results
    
    ```

    **Note:** The interface supports both the synchronous and asynchronous call methods. For simplicity, this document only provides examples of how to call important interfaces in synchronous and asynchronous modes. For other interfaces, only asynchronous call examples are provided.

-   Set whether to enable the DNS settings

    ```language-java
    ClientConfiguration conf = new ClientConfiguration();
    conf.setHttpDnsEnable(true);//The default value is "true", meaning enable. To disable the feature, set the value to "false".
    
    ```

-   Set the custom user-agent

    ```language-java
    ClientConfiguration conf = new ClientConfiguration();
    //The set ua is added to the end of the default ua of the SDK by default. Use the last ua value.
    conf.setUserAgentMark("customUserAgent");
    
    ```


