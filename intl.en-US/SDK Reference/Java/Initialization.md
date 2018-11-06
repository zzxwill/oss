# Initialization {#concept_32010_zh .concept}

OSSClient serves as the OSS Java client to manage OSS resources such as buckets and objects. To initiate an OSS request with Java SDK, you need to initiate an OSSClient instance first and then modify the default configuration items of ClientConfiguration.

## Create an OSSClient instance {#section_ngr_tjb_kfb .section}

To create an OSSClient instance, you need to specify an endpoint. For more information about endpoints, see [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Regions and endpoints.md#) and [Bind a custom domain name](../../../../reseller.en-US/Developer Guide/Access and control/Bind a custom domain name.md#).

-   Use an OSS domain to create an OSSClient instance

    Use the following code to create an OSSClient instance with a domain assigned by OSS:

    ```language-java
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Close your OSSClient instance.
    ossClient.shutdown();
    
    ```

    You can use one or more OSSClients for your project. In other words, you can use multiple OSSClients simultaneously.

-   Use a custom domain \(CNAME\) to create an OSSClient instance

    Run the following code to create an OSSClient instance with CNAME:

    ```language-java
    String endpoint = "<yourEndpoint>";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create a ClientConfiguration instance. Modify parameters as required.
    ClientConfiguration conf = new ClientConfiguration();
    // Enable CNAME. CNAME indicates a custom domain bound to a bucket.
    conf.setSupportCname(true);
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret, conf);
    
    // Close your OSSClient instance.
    ossClient.shutdown();
    
    ```

    **Note:** ossClient.listBuckets is not available when CNAME is used.

-   Create an OSSClient instance for Apsara Stack

    Run the following code to create an OSSClient instance for Apsara Stack:

    ```language-java
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create a ClientConfiguration instance. Modify parameters as required.
    ClientConfiguration conf = new ClientConfiguration();
    // Disable CNAME.
    conf.setSupportCname(false);
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret, conf);
    
    // Close your OSSClient instance.
    ossClient.shutdown();
    
    ```

-   Use an IP address to create an OSSClient instance

    Run the following code to create an OSSClient instance with an IP address:

    ```language-java
    // In some special cases, use the IP address as an endpoint. Specify the actual IP address based on your requirements.
    String endpoint = "http://10.10.10.10";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create ClientConfiguration.
    ClientConfiguration conf = new ClientConfiguration();
    // Enable OSS access with the level-2 domain. Access with the level-2 domain is disabled by default. You need to configure this function for OSS Java SDK versions 2.1.2 or earlier because OSS Java SDK versions later than 2.1.2 automatically detect IP addresses.
    conf.setSLDEnabled(true);
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret, conf);
    
    // Close your OSSClient instance.
    ossClient.shutdown();
    
    ```

-   Use STS to create an OSSClient instance

    Run the following code to create an OSSClient instance with Security Token Service \(STS\):

    ```language-java
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String securityToken = "<yourSecurityToken>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret, securityToken);
    
    // Close your OSSClient instance.
    ossClient.shutdown();
    
    ```


For more information, see [What is RAM and STS](../../../../reseller.en-US/Best Practices/Access control/What is RAM and STS.md#) and [Authorized access](reseller.en-US/SDK Reference/Java/Authorized access.md#).

## Configure an OSSClient instance { .section}

ClientConfiguration is a configuration class of OSSClient. ClientConfiguration is used to configure parameters such as user agents, host proxies, connection timeout, and the maximum number of connections. You can configure the following parameters.

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|MaxConnections|Specifies the maximum number of HTTP connections that can be enabled. The default value is 1024.|ClientConfiguration.setMaxConnections|
|SocketTimeout|Specifies the timeout time in milliseconds for data transmission at the sockets layer. The default value is 50,000.|ClientConfiguration.setSocketTimeout|
|ConnectionTimeout|Specifies the timeout time in milliseconds when connections are established. The default value is 50,000 milliseconds.|ClientConfiguration.setConnectionTimeout|
|ConnectionRequestTimeout|Specifies the timeout time in milliseconds for obtaining connections from the connection pool. No timeout is configured by default. No timeout is configured by default.|ClientConfiguration.setConnectionRequestTimeout|
|IdleConnectionTime|Specifies the idle connection timeout time. If the idle connection time in milliseconds exceeds the specified value, the connection is closed. The default value is 60,000.|ClientConfiguration.setIdleConnectionTime|
|MaxErrorRetry|Specifies the maximum number of retry attempts in the case of a request error. The default value is 3.|ClientConfiguration.setMaxErrorRetry|
|SupportCname|Specifies whether CNAME can be used as an endpoint. You can use CNAME as an endpoint by default.|ClientConfiguration.setSupportCname|
|SLDEnabled|Specifies whether access with the level-2 domain is enabled. Access with the level-2 domain is disabled by default.|ClientConfiguration.setSLDEnabled|
|Protocol|Specifies the protocol \(HTTP or HTTPS\) used to connect to OSS. The default value is HTTP.|ClientConfiguration.setProtocol|
|UserAgent|Specifies the user agent \(the user agent in the HTTP header\). The default value is “aliyun-sdk-java.”|ClientConfiguration.setUserAgent|
|ProxyHost|Specifies the IP address to access the proxy host.|ClientConfiguration.setProxyHost|
|ProxyPort|Specifies the port for the proxy host.|ClientConfiguration.setProxyPort|
|ProxyUsername|Specifies the username verified by the proxy host.|ClientConfiguration.setProxyUsername|
|ProxyPassword|Specifies the password verified by the proxy host.|ClientConfiguration.setProxyPassword|

Run the following code to configure parameters for an OSSClient instance with ClientConfiguration:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create ClientConfiguration. ClientConfiguration is a configuration class of OSSClient. You can use ClientConfiguration to configure parameters such as user agents, host proxies, connection timeout, and the maximum number of connections.
ClientConfiguration conf = new ClientConfiguration();

// Configure the maximum HTTP connections allowed by an OSSClient instance. The default value is 1024.
conf.setMaxConnections(200);
// Configure the timeout time in milliseconds for data transmission at SSL. The default value is 50,000.
conf.setSocketTimeout(10000);
conf.setSocketTimeout(10000);
// Configure the timeout time in milliseconds when connections are established. The default value is 50,000.
conf.setConnectionTimeout(10000);
// Configure the timeout time in milliseconds for retrieving connections from the connection pool. This function is disabled by default.
conf.setConnectionRequestTimeout(1000);
// Configure idle connection timeout. If the idle connection time in milliseconds exceeds the specified value, the connection is closed. The default value is 60,000.
conf.setIdleConnectionTime(10000);
// Configure the maximum number of retry attempts in the case of a request error. The default value is 3.
conf.setMaxErrorRetry(5);
// Check whether CNAME can be used as an endpoint. You can use CNAME as an endpoint by default.
conf.setSupportCname(true);
// Check whether access with the level-2 domain is enabled. Access with the level-2 domain is disabled by default.
conf.setSLDEnabled(true);
// Configure the protocol (HTTP or HTTPS) used to connect to OSS. HTTP is used by default.
conf.setProtocol(Protocol.HTTP);
// Configure the user agent (the user agent in the HTTP header). The default value is "aliyun-sdk-java."
conf.setUserAgent("aliyun-sdk-java");
// Configure the port for the proxy host.
conf.setProxyHost("<yourProxyHost>");
// Configure the username verified by the proxy host.
conf.setProxyUsername("<yourProxyUserName>");
// Configure the password verified by the proxy host.
conf.setProxyPassword("<yourProxyPassword>");

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret, conf);

// Close your OSSClient instance.
ossClient.shutdown();

```

