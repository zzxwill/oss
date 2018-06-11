# Set up data callback for mobile apps {#concept_jqr_s1y_5db .concept}

## Background {#section_jrq_t1y_5db .section}

[Setting up direct data transfer for mobile apps](intl.en-US/Best Practices/Application server/Set up direct data transfer for mobile apps.md#) describes how to set up OSS-based direct data transfer for mobile apps in 30 minutes. The following flowchart describes mobile app development:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4401/1424_en-US.png)

Role:

-   The app server generates an STS credential for the Android/iOS mobile app.

-   The Android/iOS mobile app applies for the STS credential from the app server and then uses the STS credential.

-   The OSS processes requests from the Android/iOS mobile app.


After performing Step 1 \(apply for an STS credential\) in the preceding flowchart, the Android/iOS mobile app, can perform Step 5 \(use the STS credential to upload data to OSS\) repeatedly. In this case, the app server does not know what data the app is uploading, and the app developer cannot manage the uploaded data. Is there any way to make the app server be aware of the data uploaded by the Android/iOS mobile app?

In this case, the OSS data callback service can be used to tackle these type of issues. You can see the following flowchart:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4401/1426_en-US.png)

OSS triggers a callback after receiving data from the Android/iOS mobile app \(Step 5 in the preceding flowchart\) but before returning the upload result to the app \(Step 6\). The callback is marked as Step 5.5. OSS calls back data from the app server and obtains the content returned by the app server. Then OSS returns the content to the Android/iOS mobile app. For more information, see [Callback  API Documentation](../intl.en-US/API Reference/Object operations/Callback.md#).

## Data callback function {#section_bnl_jby_5db .section}

-   Retrieving basic information about the data uploaded to the app server

    The following table shows the basic information. One or more of the following variables are returned, and the format of returned content is specified when the Android/iOS mobile app uploads data.

    |System variable|Meaning|
    |:--------------|:------|
    |bucket|Storage space \(bucket\) to which the mobile app uploads data|
    |object|File name saved on OSS for the data uploaded by the mobile app|
    |etag|etag of the uploaded file. It is the etag field returned to the mobile app|
    |size|Size of the uploaded file|
    |mimeType|Resource type|
    |imageInfo.height|Image height|
    |imageInfo.width|Image width|
    |imageInfo.format|Image format, for example, JPG and PNG \(only for recognized images\)|

-   Transferring information through custom variables

    If you are a developer and want to know the app version, OS version, location, and mobile phone model of the user who is uploading data, you can specify the Android/iOS mobile app client to send the preceding variables when uploading files. For example,

    -   x:version indicates app version.

    -   x:system indicates OS version.

    -   x:gps indicates location.

    -   x:phone indicates mobile phone model.

        These values are attached when the Android/iOS mobile app uploads data to OSS. Then OSS includes the values in the CallbackBody and sends them to the app server. In this way, the information is transferred to the app server.


## Data callback setup for the mobile app client {#section_hvc_5by_5db .section}

To enable OSS to trigger a callback when receiving an upload request, the mobile app must include the following two items in the request:

-   callbackUrl indicates the app server to which data is called back, for example,  `http://abc.com/callback.php`. Note that the server address must be accessible through the Internet.
-   callbackBody indicates the content to be called back and sent to the app server. The content can include one or more of the variables OSS returns to the app server.

For example, assume that the data is called back and sent to the app server at `http://abc.com/callback.php`. You want to obtain the name and size of the file uploaded by the mobile phone. The defined variable "photo" gets the mobile phone model, and the variable "system" gets the OS version.

Two samples of upload callbacks are listed as follows:

-   Data callback sample code for iOS apps:

    ```
    OSSPutObjectRequest * request = [OSSPutObjectRequest new];
    request.bucketName = @"<bucketName>";
    request.objectKey = @"<objectKey>";
    request.uploadingFileURL = [NSURL fileURLWithPath:@<filepath>"];
    // Set callback parameters
    request.callbackParam = @{
                              @"callbackUrl": @"http://abc.com/callback.php",
                              @"callbackBody": @"filename=${object}&size=${size}&photo=${x:photo}&system=${x:system}"
                              
    // Set custom variables
    request.callbackVar = @{
                            @"x:photo": @"iphone6s",
                            @"x:system": @"ios9.1"
                            
    ```

-   Data callback sample code for Android apps:

    ```
    PutObjectRequest put = new PutObjectRequest(testBucket, testObject, uploadFilePath);
    ObjectMetadata metadata = new ObjectMetadata();
    metadata.setContentType("application/octet-stream");
    put.setMetadata(metadata);
    put.setCallbackParam(new HashMap<String, String>() {
        
            put("callbackUrl", "http://abc.com/callback.php");
            put("callbackBody", "filename=${object}&size=${size}&photo=${x:photo}&system=${x:system}");
        
    
    put.setCallbackVars(new HashMap<String, String>() {
         
             put("x:photo", "IPOHE6S");
             put("x:system", "YunOS5.0");
         
    
    ```


## Data callback requirements for the app server {#section_adb_rcy_5db .section}

-   You must deploy a service for receiving POST requests. This service must have a public address, for example, `www.abc.com/callback.php` \(or an Internet IP address\); otherwise, OSS cannot access this address.

-   You must set the format of custom content returned to OSS to JSON. OSS delivers the content received from the app server as it is to the Android/iOS mobile app. \(The Response header returned to OSS must carry the Content-Length header.\)


The last section provides sample callback programs based on multiple programming languages, together with the download links and running methods.

## Callback request received by the app server {#section_qmv_scy_5db .section}

The packet of a callback request the app server receives from OSS is as follows \(the data varies with different URLs and callback content\):

```
POST /index.html HTTP/1.0
Host: 121.43.113.8
Connection: close
Content-Length: 81
Content-Type: application/x-www-form-urlencoded
User-Agent: ehttp-client/0.0.1
authorization: kKQeGTRccDKyHB3H9vF+xYMSrmhMZjzzl2/kdD1ktNVgbWEfYTQG0G2SU/RaHBovRCE8OkQDjC3uG33esH2txA==
x-oss-pub-key-url: aHR0cDovL2dvc3NwdWJsaWMuYWxpY2RuLmNvbS9jYWxsYmFja19wdWJfa2V5X3YxLnBlbQ==
filename=test.txt&size=5&photo=iphone6s&system=ios9.1
```

For more information, see [Callback API Documentation](../intl.en-US/API Reference/Object operations/Callback.md#).

## How does the app server determine whether a callback request is sent by OSS? {#section_ewm_hdy_5db .section}

The app server must determine whether a callback request is from OSS because the app server may receive invalid requests that affect its normal logic when the app server has a malicious callback during a network attack.

To determine request validity, the app server verifies the RSA checksum using the `x-oss-pub-key-url` and `authorization` parameters in the content OSS sends to the app server. Only requests that pass RSA checksum verification are sent by OSS. The sample programs in this document also provides implementation results, for your reference.

## How does the app server process the received callback request? {#section_nj2_jdy_5db .section}

After verifying a request from OSS, the app server processes the request based on its content. The Android/iOS mobile app specifies the format of the callback content when uploading the data, for example:

```
filename=test.txt&size=5&photo=iphone6s&system=ios9.1
```

The app server parses the OSS-returned content to obtain the expected data. Then the app server stores the data for subsequent management.

## How does the app server return the callback request to OSS? {#section_wzh_pdy_5db .section}

-   The returned status code is 200.
-   The returned content must use the JSON format.
-   The returned content must carry the Content-Length header.

## How does OSS process the content returned by the app server? {#section_oz5_rdy_5db .section}

There are two scenarios:

-   In case that the app server fails to receive the callback request or is not accessible, OSS returns a 203 status code to the Android/iOS mobile app. However, the uploaded data is already saved to OSS.

-   If the app server receives a callback request and returns the correct status code, OSS returns content received from the app server as it is to the Android/iOS mobile app along with a 200 status code.


## Sample callback programs for downloading {#section_vs4_z2y_5db .section}

The sample program shows how to check the signature received by the application server. You must add the code for parsing the format of the callback content received by the application server.

-   Java version:
    -   [Download address](https://gosspublic.alicdn.com/images/AppCallbackServer.zip).
    -   Running method: Extract the archive and run `java -jar oss-callback-server-demo.jar 9000` \(9000 is the port number and can be changed as required\).

        **Note:** This jar runs on java 1.7.  If any problem occurs, you may make changes based on the provided code. This is a maven project.

-   PHP version:
    -   [Download address](https://gosspublic.alicdn.com/callback-php-demo.zip).
    -   Running method: Deploy the program to an Apache environment. Due to the characteristics of the PHP language, retrieving headers depends on the environment. You may make modifications to the example based on your own environment.
-   Python version:
    -   [Download address](https://gosspublic.alicdn.com/images/callback_app_server.py.zip).
    -   Running method: Extract the archive and directly run python callback\_app\_server.py. The program implements a simple HTTP server. To run this program, you may need to install the system environment on which the RSA depends.
-   Ruby version:
    -   [Download address](https://github.com/rockuw/oss-callback-server).
    -   Running method: ruby aliyun\_oss\_callback\_server.rb

