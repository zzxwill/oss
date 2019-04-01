# Quick start {#concept_32045_zh .concept}

This topic is the quick start for the OSS Android SDK.

The basic file upload and download processes are demonstrated as follows. For more information, see the following directories of this project:

sample directory: Demos of uploading local files, download objects, resumable downloads, and configuring callback are included. [Click to view details](https://github.com/aliyun/aliyun-oss-android-sdk/tree/master/app).

You can also run git clone [project](https://github.com/aliyun/aliyun-oss-android-sdk.git) and configure the necessary parameters as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22517/155409020313686_en-US.png)

Then run the demo as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22517/155409020313687_en-US.png)

1.  Initialize the OSSClient.

    The initialization process mainly includes the following steps: endpoint settings, authentication mode settings, and client parameter settings. Three authentication modes are available: plain text setting mode, self-signed mode, and STS authentication mode. To use the STS authentication, see `Resource Access Management` to learn more about the RAM. Assuming that you have activated the RAM service and learned the RAM service and how to obtain sub-account AccessKeyId, SecretKeyId, and RoleArn.

    Complete the parameter information of AccessKeyId, SecretKeyId, and RoleArn in the [script files](https://github.com/aliyun/aliyun-oss-android-sdk/blob/master/app/sts_local_server/python/sts.py). You can enable a local HTTP service with Python. Access local services using the client codes to obtain the StsToken.AccessKeyId, StsToken.SecretKeyId, and StsToken.SecurityToken.

    For more information, see STS instructions in the sample. [Click to view details](https://github.com/aliyun/aliyun-oss-android-sdk/tree/master/app/src/main/java/com/alibaba/sdk/android/oss/app).

    ```language-java
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    
    // We recommend that you initialize OSSClient using STS on the mobile device.
    // For more information, see STS instructions in the sample (https://github.com/aliyun/aliyun-oss-ios-sdk/tree/master/DemoByOC).
    OSSCredentialProvider credentialProvider = new OSSStsTokenCredentialProvider("<StsToken.AccessKeyId>", "<StsToken.SecretKeyId>", "<StsToken.SecurityToken>");
    
    //If this configuration class is not set, default configuration will apply. For details, see description of the class.
    ClientConfiguration conf = new ClientConfiguration();
    conf.setConnectionTimeout(15 * 1000); // Connection time-out. The default value is 15 seconds.
    conf.setSocketTimeout(15 * 1000); // Socket time-out. The default value is 15 seconds.
    conf.setMaxConcurrentRequest(5); // The maximum number of concurrent requests. The default value is 5.
    conf.setMaxErrorRetry(2); // The maximum number of retry attempts after each failed attempt. The default value is 2.
    
    //When the feature is enabled, you can view logs in the console and write a log file into the mobile phone SD card under SDCard_path\OSSLog\logs.csv. This feature is disabled by default.
    //The log will record the request data, returned data and exceptions of OSS operations.
    //Such as requestId and response header
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
    OSSLog.enableLog();
    
    OSS oss = new OSSClient(getApplicationContext(), endpoint, credentialProvider);
    
    ```

    Initialization of upload and download requests using the OSSClient is thread-safe. You can run multiple tasks concurrently.

2.  Upload a file

    Assuming that you already have a bucket in the OSS console. An OSSTask is returned for each operation related to SDK. You can configure a continuation action for the task or call the waitUntilFinished block to wait until the task completes.

    ```language-java
    // Construct an upload request.
    PutObjectRequest put = new PutObjectRequest("<bucketName>", "<objectKey>", "<uploadFilePath>");
    
    // You can set progress callback during the asynchronous upload
    put.setProgressCallback(new OSSProgressCallback<PutObjectRequest>() {
      @Override
      public void onProgress(PutObjectRequest request, long currentSize, long totalSize) {
        Log.d("PutObject", "currentSize: " + currentSize + " totalSize: " + totalSize);
      }
    });
    
    OSSAsyncTask task = oss.asyncPutObject(put, new OSSCompletedCallback<PutObjectRequest, PutObjectResult>() {
      @Override
      public void onSuccess(PutObjectRequest request, PutObjectResult result) {
        Log.d("PutObject", "UploadSuccess");
      }
    
      @Override
      public void onFailure(PutObjectRequest request, ClientException clientExcepion, ServiceException serviceException) {
        // Request exception
        if (clientExcepion ! = null) {
          // Local exception, such as a network exception
          clientExcepion.printStackTrace();
        }
        if (serviceException ! = null) {
          // Service exception
          Log.e("ErrorCode", serviceException.getErrorCode());
          Log.e("RequestId", serviceException.getRequestId());
          Log.e("HostId", serviceException.getHostId());
          Log.e("RawMessage", serviceException.getRawMessage());
        }
      }
    });
    
    // task.cancel(); // You can cancel the task.
    
    // task.waitUntilFinished(); // Wait until the task is complete.
    
    ```

3.  Download a specified object.

    The following code downloads the specified `object`, \(you need to handle the input stream of the returned data\):

    ```language-java
    // Construct a download request.
    GetObjectRequest get = new GetObjectRequest("<bucketName>", "<objectKey>");
    
    OSSAsyncTask task = oss.asyncGetObject(get, new OSSCompletedCallback<GetObjectRequest, GetObjectResult>() {
      @Override
      public void onSuccess(GetObjectRequest request, GetObjectResult result) {
        // Request succeeds
        Log.d("Content-Length", "" + getResult.getContentLength());
    
        InputStream inputStream = result.getObjectContent();
    
        byte[] buffer = new byte[2048];
        int len;
    
        try {
          while ((len = inputStream.read(buffer)) ! = -1) {
            // Process the downloaded data.
          }
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    
      @Override
      public void onFailure(GetObjectRequest request, ClientException clientExcepion, ServiceException serviceException) {
        // Request exception
        if (clientExcepion ! = null) {
          // Local exception, such as a network exception.
          clientExcepion.printStackTrace();
        }
        if (serviceException ! = null) {
          // Service exception
          Log.e("ErrorCode", serviceException.getErrorCode());
          Log.e("RequestId", serviceException.getRequestId());
          Log.e("HostId", serviceException.getHostId());
          Log.e("RawMessage", serviceException.getRawMessage());
        }
      }
    });
    
    // task.cancel(); // You can cancel the task.
    
    // task.waitUntilFinished(); // Wait until the task is complete if necessary.
    
    
    ```


