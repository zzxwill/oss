# Initialization {#concept_32087_zh .concept}

OSSClient serves as the OSS C\# client to manage OSS resources such as buckets and objects.

## Create an OSSClient instance {#section_yfv_czd_lfb .section}

To create an OSSClient instance, you need to specify an endpoint. For more information about endpoints, see [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Regions and endpoints.md#) and [Bind a custom domain name](../../../../reseller.en-US/Developer Guide/Access and control/Bind a custom domain name.md#).

-   Use an OSS domain to create an OSSClient instance

    Run the following code to create an OSSClient instance with a domain assigned by OSS:

    ```language-csharp
    using Aliyun.OSS;
    
    const string accessKeyId = "<yourAccessKeyId>";
    const string accessKeySecret = "<yourAccessKeySecret>";
    const string endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    
    // Create a new OSSClient instance with the OSS access address specified by the user and the AccessKeyId/AccessKeySecret granted by Alibaba Cloud.
    var ossClient = new OssClient(endpoint, accessKeyId, accessKeySecret);
    
    
    ```

-   Use a custom domain \(CNAME\) to create an OSSClient instance

    Run the following code to create an OSSClient instance with CNAME:

    ```language-csharp
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
    
    const string accessKeyId = "<yourAccessKeyId>";
    const string accessKeySecret = "<yourAccessKeySecret>";
    const string endpoint = "<yourDomain>";
    
    // Create a ClientConfiguration instance. Modify parameters as required.
    var conf = new ClientConfiguration();
    
    // Enable CNAME. CNAME indicates a custom domain bound to a bucket.
    conf.IsCname = true;
    
    // Create an OSSClient instance.
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret, conf);
    
    ```

    **Note:** The ossClient.listBuckets method cannot be used when a CNAME is used.


## Configure an OSSClient instance { .section}

ClientConfiguration is a configuration class of OSSClient. ClientConfiguration is used to configure parameters such as user agents, host proxies, connection timeout, and the maximum number of connections. You can configure the following parameters.

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|ConnectionLimit|Specifies the maximum number of HTTP connections that can be enabled.|512|
|MaxErrorRetry|Specifies the maximum number of retry attempts in the case of a request error.|3|
|ConnectionTimeout|Specifies the timeout time in milliseconds when connections are established. The default value is -1, indicating that the connections are not time-out.|-1|
|IsCname|Specifies whether CNAME can be used as an endpoint.|false|
|ProgressUpdateInterval|Specifies the update interval of the progress bar, which is calculated by bytes.|8096|

A code example is given as follows:

```language-csharp
using Aliyun.OSS;
using Aliyun.OSS.Common;

var conf = new ClientConfiguration();
conf.ConnectionLimit = 512;
conf.MaxErrorRetry = 3;
conf.ConnectionTimeout = 300;

var client = new OssClient(endpoint, accessKeyId, accessKeySecret, conf);

```

-   Data verification

    Run the following code for MD5 data verification:

    ```language-csharp
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
    
    var conf = new ClientConfiguration();
    // Enable MD5 verification. Set EnableMD5Check to perform MD5 verification on the uploaded or downloaded data. MD5 verification is disabled by default.
    conf.EnalbeMD5Check = true;
    
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret, conf);
    
    ```

    **Note:** System performance reduces when MD5 verification is disabled.

-   Proxy network

    If you use proxy networks, you can configure the following parameters to access OSS:

    |Parameter|Description|Default value|
    |:--------|:----------|:------------|
    |ProxyHost|A proxy server, such as `8.8.8.8` or `abc.def.com`.|Null|
    |ProxyPort|A proxy port, such as `3128` or `8080`.|Null|
    |ProxyUserName|The proxy service account, which is optional|Null|
    |ProxyPassword|The proxy service password, which is optional|Null|

    An example of proxy network access without using the account and password is as follows:

    ```language-csharp
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
    
    var conf = new ClientConfiguration();
    conf.ProxyHost = "8.8.8.8";
    conf.ProxyPort = 3128;
    
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret, conf);
    
    ```

    An example of proxy network access using the account and password is as follows:

    ```language-csharp
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
    
    var conf = new ClientConfiguration();
    conf.ProxyHost = "8.8.8.8";
    conf.ProxyPort = 3128;
    conf.ProxyUserName = "user";
    conf.ProxyPassword = "6666";
    
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret, conf);
    
    ```


