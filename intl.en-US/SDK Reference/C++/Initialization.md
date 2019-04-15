# Initialization {#concept_32133_zh .concept}

OSSClient is used to manage OSS resources such as buckets and objects. To use the C++ SDK to initiate a request, you must initialize an OSSClient instance and modify the default configuration items of ClientConfiguration as needed.

## Configure the OSSClient instance {#section_hty_tpt_xgb .section}

ClientConfiguration is a configuration class of OSSClient. You can use ClientConfiguration to configure parameters such as user agents, host proxies, connection timeout, and the maximum number of connections. You can configure the following parameters.

|Name|Description|
|:---|:----------|
|userAgent|The user agent \(the User-Agent field in the HTTP header\). The default value is aliyun-sdk-cpp/1. X.X|
|maxConnections|The maximum number of connection pools. The default value is 16.|
|requestTimeoutMs|The request timeout period. The connection is closed if no data is received during the timeout period. The default value is 10,000 ms.|
|connectTimeoutMs|The timeout period to establish a connection. The default value is 5,000 ms.|
|retryStrategy|The maximum number of retry attempts in the case of a request error.|
|proxyScheme|The proxy protocol. The default value is HTTP.|
|proxyPort|The port for the proxy host.|
|proxyPassword|The password for proxy host verification.|
|proxyUserName|The username for proxy host verification.|
|verifySSL|Indicates whether to enable SSL verification. SSL verification is disabled by default.|
|enableCrc64|Indicates whether to enable the CRC64 check. The CRC64 check is enabled by default.|
|enableDateSkewAdjustment|Indicates whether to enable automatic correction of HTTP request time. This function is enabled by default.|
|sendRateLimiter|The limit to the upload speed, in Kbit/s.|
|recvRateLimiter|The limit to the download speed, in Kbit/s.|

## Configure a timeout period {#section_d1l_vvx_pgb .section}

Use the following code to configure a timeout period:

```
#include <alibabacloud/oss/OssClient.h>

using namespace AlibabaCloud::OSS;

int main(void)
{

    /* Initialize network resources. */
    InitializeSdk();
    ClientConfiguration conf;
  
    /* Set the maximum number of connection pools. The default value is 16. */
    conf.maxConnections = 20;
  
    /* Configure the request timeout period. If no data is received during the period, the connection is closed. The default value is 10,000 ms. */
    conf.requestTimeoutMs = 8000;
  
    /* Configure the timeout period in milliseconds when connections are established. The default value is 5,000 ms. */
    conf.connectTimeoutMs = 8000;

    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Configure speed limits {#section_whm_rtx_kfb .section}

Use the following code to configure speed limits for uploads and downloads:

```
#include <alibabacloud/oss/OssClient.h>
#include <alibabacloud/oss/client/RateLimiter.h>

using namespace AlibabaCloud::OSS;

class  UserRateLimiter : public RateLimiter
{
public:
    DefaultRateLimiter() :rate_(0) {};
    ~DefaultRateLimiter() {};
    virtual void setRate(int rate) { rate_ = rate; };
    virtual int Rate() const { return rate_; };
private:
    int rate_;
};

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";
  
    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
  
    auto sendrateLimiter = std::make_shared<UserRateLimiter>();
    auto recvrateLimiter = std::make_shared<UserRateLimiter>();
    conf.sendRateLimiter = sendrateLimiter;
    conf.recvRateLimiter = recvrateLimiter;

    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
    /* Set the speed limit for the download , in Kbit/s. */
    recvrateLimiter->setRate(256);
  
    /* Set the speed limit for the upload, in Kbit/s. */
    sendrateLimiter->setRate(256);
  
    /* Upload the file. */
    auto outcome = client.PutObject(BucketName, ObjectName,"yourLocalFilename");  
  
    /* Update the speed limit during the upload, in Kbit/s. */
    sendrateLimiter->setRate(300);
  
    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Configure a retry policy {#section_b1l_vvx_pgb .section}

Use the following code to configure a retry policy:

```
#include <alibabacloud/oss/OssClient.h>
#include <alibabacloud/oss/client/RetryStrategy.h>

using namespace AlibabaCloud::OSS;

class UserRetryStrategy : public RetryStrategy
{
public:

    /* maxRetries indicates the maximum number of retry attempts. scaleFactor indicates the factor that is used to calculate the waiting time of a retry attempt. */
    DefaultRetryStrategy(long maxRetries = 3, long scaleFactor = 300) :
        m_scaleFactor(scaleFactor), m_maxRetries(maxRetries)  
    {}
    
    /* You can define the shouldRetry function to determine whether to retry a request.*/
    bool shouldRetry(const Error & error, long attemptedRetries) const;
    
    /* You can define the calcDelayTimeMs function to calculate the waiting time (delay) of a retry attempt. */
    long calcDelayTimeMs(const Error & error, long attemptedRetries) const;
    
private:
    long m_scaleFactor;
    long m_maxRetries;
};

bool DefaultRetryStrategy::shouldRetry(const Error & error, long attemptedRetries) const
{    
    if (attemptedRetries >= m_maxRetries)
        return false;

    long responseCode = error.Status();

    //http code
    if ((responseCode == 403 && error.Message().find("RequestTimeTooSkewed")) ||
        (responseCode > 499 && responseCode < 599)) {
        return true;
    }
    else {
        switch (responseCode)
        {
        //curl error code
        case (ERROR_CURL_BASE + 7):  //CURLE_COULDNT_CONNECT
        case (ERROR_CURL_BASE + 18): //CURLE_PARTIAL_FILE
        case (ERROR_CURL_BASE + 23): //CURLE_WRITE_ERROR
        case (ERROR_CURL_BASE + 28): //CURLE_OPERATION_TIMEDOUT
        case (ERROR_CURL_BASE + 52): //CURLE_GOT_NOTHING
        case (ERROR_CURL_BASE + 55): //CURLE_SEND_ERROR
        case (ERROR_CURL_BASE + 56): //CURLE_RECV_ERROR
            return true;
        default:
            break;
        };
    }

    return false;
}

long DefaultRetryStrategy::calcDelayTimeMs(const Error & error, long attemptedRetries) const
{
    UNUSED_PARAM(error);
    return (1 << attemptedRetries) * m_scaleFactor;
}

int main(void)
{
    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
 
    /* The maximum number of retry attempts in the case of a request error. The default value is 3. */
    auto defaultRetryStrategy = std::make_shared<UserRetryStrategy>(5);
    conf.retryStrategy = defaultRetryStrategy;

    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

