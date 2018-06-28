# Use the setup application server {#concept_arq_jmf_vdb .concept}

This document elaborates, how to use a mobile app like OssDemo to access the application server for the purpose of uploading data to OSS without the need of storing AccessKeyId and AccessKeySecret to the app.

## Logic of calling {#section_rds_kmf_vdb .section}

1.  OssDemo sends a request to the address of the sts\_server received by OssDemo.
2.  sts\_server returns AccessKeyId, AccessKeySecret, SecurityToken, and Expiration to OssDemo.
3.  After receiving such information, OssDemo calls SDK and creates an OssClient instance.

## Code {#section_ofy_lmf_vdb .section}

1.  Generate an EditText control.

    ```
    Location:
     res/layout/content_main.xml
     Content:
     <EditText
         android:layout_height="wrap_content"
         android:layout_width="0dp"
         android:layout_weight="4"
         android:id="@+id/sts_server"
         android:text="@string/sts_server"
         />
     Location:
     res/values/strings
     Content:
     <string name="sts_server">http://oss-demo.aliyuncs.com/app-server/sts.php</string>
    ```

2.  Get the codes of corresponding STS parameters from the application server. 

    Function implementation:

    ```
    OSSFederationToken getFederationToken()
    ```

3.  Get the STS returned parameters and initialize the OssClient code. 

    Function implementation:

    ```
    //Initialize an OssService used for uploading and downloading.
     public OssService initOSS(String endpoint, String bucket, ImageDisplayer displayer) {
         //If you want to directly use the accessKey for access purposes, you can directly use OSSPlainTextAKSKCredentialProvider for authentication.
         //OSSCredentialProvider credentialProvider = new OSSPlainTextAKSKCredentialProvider(accessKeyId, accessKeySecret);
         //Use your own class to retrieve an STSToken
         OSSCredentialProvider credentialProvider = new STSGetter(stsServer);
         ClientConfiguration conf = new ClientConfiguration();
         conf.setConnectionTimeout(15 * 1000); // Connection timeout, 15 seconds by default
         conf.setSocketTimeout(15 * 1000); // Socket timeout, 15 seconds by default
         conf.setMaxConcurrentRequest(5); // Maximum concurrent requests, 5 by default
         conf.setMaxErrorRetry(2); // Maximum error retries, 2 by default
         OSS oss = new OSSClient(getApplicationContext(), endpoint, credentialProvider, conf);
         return new OssService(oss, bucket, displayer);
     }
    ```


