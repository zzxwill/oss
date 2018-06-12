# Set up direct data transfer for mobile apps {#concept_kxc_brw_5db .concept}

## Background {#section_udp_qrw_5db .section}

In the era of mobile Internet, mobile apps upload more and more data every day. By handing off their data storage issues to OSS, developers can focus more on their app logic.

This article describes how to set up an OSS-based direct data transfer service for a mobile app in 30 minutes. Direct data transfer is a service that allows a mobile app to directly connect to OSS for data upload and download, while only sending the control traffic to the app server.

## Advantages {#section_gl2_srw_5db .section}

Setting up an OSS-based direct data transfer service for a mobile app offers the following advantages:

-   More secure upload/download method \(temporary and flexible permission assignment and authentication\).
-   Low cost. Fewer app servers. The mobile app is directly connected to the cloud storage and only the control traffic is sent to the app server.
-   High concurrency and support for a massive amount of users \(OSS has massive bandwidth for uploading and downloading use\).
-   Elasticity \(OSS’s storage space can be expanded unlimitedly\).
-   Convenience. You can easily connect to the MTS -video multiport adapter, Image Service, CDN download acceleration, and other services.

The architecture diagram is as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4399/6269_en-US.png)

Details:

-   Android/iOS mobile app, which is the app installed on the end user's mobile phone.
-   OSS, short for Alibaba Cloud Object Storage Service, which stores app-uploaded data. For more information, see [OSS description on Alibaba Cloud website](http://www.aliyun.com/product/oss).
-   RAM/STS, which generates temporary access credentials.
-   App server, which is the background service developed for the Android/iOS mobile app and used to manage the tokens used for data uploading/downloading by the app and the metadata of the app-uploaded data.

## Steps: {#section_cyl_rsw_5db .section}

1.  Request for a temporary upload credential from the app server.

    The Android/iOS app cannot store AccessKeyID/AccessKeySecret directly, which may cause the risk of information leakage.  Therefore, the app must request a temporary upload credential \(a token\) from the app server.  The token is only valid for a certain period. For example, if a token is set to be valid for 30 minutes \(editable by the app server\), then the Android/iOS app can use this token to upload/download data to/from the OSS within the next 30 minutes.  30 minutes later, the app must request a new token to upload/download data.

2.  The app server checks the validity of the preceding request and then returns a token to the app.
3.  After the cell phone receives this token, it can upload or download data from the OSS.

This article mainly describes the content in the red circle and blue circle of the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4399/6270_en-US.png)

-   The blue circle shows how the app server generates a token.
-   The red circle shows how the Android/iOS app receives the token.

Effect:

You can scan the QR code to install the sample app, as shown in the following figure. This tool is developed on Android. The app server in this document can also be used on iOS.

The interface for connecting the sample app to the OSS is shown in the following figure:

**Note:** The address displayed in the app server is a sample address. You can deploy the app server on your own by referring to the STS app server code at the end of the article.

-   App Server: the background server corresponding to this mobile app.

-   Upload Bucket: the bucket to which the mobile app will upload data.

-   Region: the region in which a bucket is uploaded.


Steps for using the sample app:

-   Click**Select image** and upload an object to the OSS.

-   You can choose normal upload or resumable upload.

    **Note:** Resumable upload is recommended in poor network environments. You can use Image Service to scale down and add a watermark to the image to be uploaded. Do not modify the server URL and bucket name in initial use.


## Prerequisites for setting up direct data transfer service {#section_x15_j5w_5db .section}

Preparations for setting up direct data transfer service:

1.  [Activate the OSS service](../intl.en-US/Quick Start/Sign up for OSS.md#) and [create a bucket](../intl.en-US/Developer Guide/Bucket Management/Create a bucket.md#).
2.  Activate the STS service.
    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
    2.  On the OSS **Overview** page, find the Basic Settings area,  and click **Security Token**, as shown in the following figure.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4399/6271_en-US.png)

    3.  Enter the Quick Security Token Configuration page.

        **Note:** If RAM has not yet been activated, a prompt box to activate RAM appears.  Click **Activate** and perform real-name verification.  After the verification is finished, the following page appears. Click  **Start Authorization**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4399/6272_en-US.png)

    4.  The system performs authorization automatically. Be sure to save the parameters in the three red boxesoxes in the following figures.  Click **Save Access Key Information** to close the dialog box and complete STS activation.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4399/6273_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4399/6274_en-US.png)

    5.  If you have already created an AccessKeyId/AccessKessKeySecret, the following prompt window appears:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4399/6275_en-US.png)

        -   Click **View**, as shown in the following figure.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4399/6276_en-US.png)

        -   Click **Create Access Key**, as shown in the following figure.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4399/6277_en-US.png)

        -   Record parameters 1, 2, and 3, as shown in the following figure.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4399/6278_en-US.png)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4399/6279_en-US.png)

        -   Once you have saved the three parameters, STS activation is complete.

## Set up an app server {#section_otj_v1x_5db .section}

Configuration of sample app server

**Note:** The app in this example is written in PHP. You may write your app in your preferred language, e.g. Java, Python,  Go, Ruby, Node.js, or C\#.

This tutorial provides development sample programs available for download in multiple languages. The download addresses are shown at the end of this article.

The downloaded package in each language contains a configuration file named config.json.

```
{
"AccessKeyID" : "",
"AccessKeySecret" : "",
"RoleArn" : "",
"TokenExpireTime" : "900",
"PolicyFile": "policy/all_policy.txt"
}
```

**Note:** 

-   1.  AccessKeyID: Set it to parameter 1 marked with a red box in the preceding figure.
2.  AccessKeySecret: Set it to parameter 2 marked with a red box in the preceding figure.
3.  RoleArn: Set it to parameter 3 marked with a red box in the preceding figure.
4.  TokenExpireTime: indicates the expiration time of the token obtained by the Android/iOS app. The minimum value is 900s. The default value can be retained.
5.  PolicyFile: indicates the file that lists the permissions the token grants. The default value can be retained.

This document has provided three token files defining the most common permissions in the policy directory.  They are:

-   all\_policy.txt: specifying a token that grants permissions to create or delete a bucket, or upload, download, or delete a file for this account .
-   bucket\_read\_policy.txt: specifying a token that grants permission to read the specified bucket for this account.
-   bucket\_read\_write\_policy.txt: specifying a token that grants permission to read and write the specified bucket for this account.

If you want to create a token to grant read and write permissions for the specified bucket, replace $BUCKET\_NAME in the bucket\_read\_policy.txt and  bucket\_read\_write\_policy.txt files with the name of the specified bucket.

-   Explanation of the formats of returned data:

    ```
    //Correct result returned
    {
        "StatusCode":200,
        "AccessKeyId":"STS. 3p***dgagdasdg",
        "AccessKeySecret":"rpnwO9***tGdrddgsR2YrTtI",
       "SecurityToken":"CAES+wMIARKAAZhjH0EUOIhJMQBMjRywXq7MQ/cjLYg80Aho1ek0Jm63XMhr9Oc5s˙∂˙∂3qaPer8p1YaX1NTDiCFZWFkvlHf1pQhuxfKBc+mRR9KAbHUefqH+rdjZqjTF7p2m1wJXP8S6k+G2MpHrUe6TYBkJ43GhhTVFMuM3BZajY3VjZWOXBIODRIR1FKZjIiEjMzMzE0MjY0NzM5MTE4NjkxMSoLY2xpZGSSDgSDGAGESGTETqOio6c2RrLWRlbW8vKgoUYWNzOm9zczoqOio6c2RrLWRlbW9KEDExNDg5MzAxMDcyNDY4MThSBTI2ODQyWg9Bc3N1bWVkUm9sZVVzZXJgAGoSMzMzMTQyNjQ3MzkxMTg2OTExcglzZGstZGVtbzI=",
       "Expiration":"2015-12-12T07:49:09Z",
    }
    //Wrong result returned
    {
        "StatusCode":500,
        "ErrorCode":"InvalidAccessKeyId.NotFound",
        "ErrorMessage":"Specified access key is not found."
    }
    ```

    Explanation of correct result returned: \(The following five variables comprise a token\)

    -   StatusCode: The status indicates the result that the app retrieves the token. The app returns 200 for successful retrieval of the token.
    -   AccessKeyId: indicates the AccessKeyId the Android/iOS app obtains when initializing the OSS client.
    -   AccessKeySecret: indicates the AccessKeySecret the Android/iOS app obtains when initializing the OSS client.
    -   SecurityToken: indicates the token the Android/iOS app initializes.
    -   Expiration: indicates the time when the token expires.  The Android SDK automatically determines the validity of the token and then retrieves a new one as needed.
    Explanation of wrong result returned:

    -   StatusCode: The status indicates the result that the app retrieves the token. The app returns 500 for unsuccessful retrieval of the token.
    -   ErrorCode: indicates the error causes.
    -   ErrorMessage: indicates the detailed information about the error.

-   Method for running sample code:
    -   For PHP, download and unzip a pack, modify the config.json file, run php sts.php to  generate a token, and deploy the program to the specified address.

    -   For Java \(based on Java 1.7\), after downloading and unzipping a pack, 

        Run this command: java -jar oss-token-server.jar \(port\).  If you run java  –jar oss-token-server.jar without specifying a port, the program listens to Port 7080.  To change the listening port to 9000, run java –jar  app-token-server.jar 9000. Specify the port number as needed.


## How to upload files from your app to oss {#section_ttc_33x_5db .section}

1.  After setting up the app server, write down the server address, which is `http://abc.com:8080`.  Then, replace the app server address in the sample project with this address.
2.  Specify the bucket and region for the upload in the sample apps.
3.  Click **Set** to load the configuration.
4.  Select an image file, set the object name to upload to OSS, and select Upload.  Now you can experience the OSS service on Android. Data from the Android app can be uploaded directly to OSS.
5.  After the upload is complete, check that the data is on OSS.

## Explanation of core code {#section_lsy_k3x_5db .section}

OSS initialization

-   Android versions

    ```
    // We recommend you use OSSAuthCredentialsProvider because it automatically updates expired tokens.
    String stsServer = "http://abc.com:8080 is an example of an application server address."
    OSSCredentialProvider credentialProvider = new OSSAuthCredentialsProvider(stsServer);
    //config
    ClientConfiguration conf = new ClientConfiguration();
    conf.setConnectionTimeout(15 * 1000); // Connection time-out. The default value is 15 seconds.
    conf.setSocketTimeout(15 * 1000); // Socket time-out. The default value is 15 seconds.
    conf.setMaxConcurrentRequest(5); // The maximum number of concurrent requests. The default value is 5.
    conf.setMaxErrorRetry(2); // The maximum number of retry attempts after each failed attempt. The default value is 2.
    OSS oss = new OSSClient(getApplicationContext(), endpoint, credentialProvider, conf);
    ```


-   iOS version

    ```
    OSSClient * client;
    ...
    // We recommend you use OSSAuthCredentialProvider because it automatically updates expired tokens.
    id<OSSCredentialProvider> credential = [[OSSAuthCredentialProvider alloc] initWithAuthServerUrl:@"application server address, such as http://abc.com:8080"];
    client = [[OSSClient alloc] initWithEndpoint:endPoint credentialProvider:credential];
    ```


## Download source code {#section_evy_x3x_5db .section}

Example program

-   Sample app source code for Android: [download address](https://github.com/aliyun/aliyun-oss-android-sdk?spm=a2c4g.11186623.2.9.FXb5vt)
-   Sample app source code for iOS: [download address](https://github.com/aliyun/aliyun-oss-ios-sdk?spm=a2c4g.11186623.2.10.FXb5vt)

Download sample code of app server

-   PHP: [download address](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31920/cn_zh/1510638617750/sts-server.zip?spm=a2c4g.11186623.2.11.FXb5vt&file=sts-server.zip)
-   Java: [download address](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31920/cn_zh/1510638794201/AppTokenServerDemo.zip?spm=a2c4g.11186623.2.12.FXb5vt&file=AppTokenServerDemo.zip)
-   Ruby: [download address](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31920/cn_zh/1510645887259/sts-app-server-master.zip?spm=a2c4g.11186623.2.13.FXb5vt&file=sts-app-server-master.zip)
-   node.js: [download address](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31920/cn_zh/1510647848681/sts-app-server-node.zip?spm=a2c4g.11186623.2.14.FXb5vt&file=sts-app-server-node.zip)

