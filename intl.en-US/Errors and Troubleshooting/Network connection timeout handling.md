# Network connection timeout handling {#concept_lhz_qvt_4fb .concept}

## Introduction {#section_bww_r22_qfb .section}

Network connection timeout is a typical problem that OSS SDK users may encounter when they upload files with the SDK. In such a case, a ConnectionTimeOut error is reported, negatively affecting user experience.

## Troubleshooting {#section_hzl_v22_qfb .section}

Possible causes are described as follows to analyze and resolve the network connection timeout problem of OSS SDK because this problem cannot be reproduced.

1.  Network environment

    Analyze the following network link:

    Mobile phone/PC --- Carrier network --- OSS server

    Your network may be at an edge node of the carrier network. Therefore, the requests sent to the carrier network are more likely to fail. You can use CDN edge nodes for acceleration, reducing the dependency of mobile phones/PCs on the carrier network. The network link is as follows:

    Mobile phone/PC -- Nearest CDN edge node -- Carrier network -- OSS Server

    If the problem still exists and the ConnectionTimeOut error still occurs, read the following analysis.

2.  Network configuration

    The following code is the detailed timeout error message:

    ```
    "ConnectionTimeoutError&errormsg=Failed to upload some parts with error: ConnectionTimeoutError: Connect timeout for 60000ms, PUT https://***.oss-cn-hangzhou.aliyuncs.com/***/***/***.mp4?partNumber=2&uploadId=*** -2 (connected: false, keepalive socket: false)headers: {} part_num: 2
    ```

    The following conclusions can be drawn from the error message:

    -   The connection because the client does not receive a response from the server in 60 seconds.
    -   According to CDN logs, the timeout problem occurs because the network is disconnected before a part is completely uploaded.
    -   In poor network conditions, the client/PC cannot receive responses from the OSS server in a long time if the file to be uploaded is too large.
    Based on the preceding conclusions, we recommend the following solutions:

    -   Upload files with the multipart upload method and limit the maximum part size to 1 MB.
    -   Add a resumable mechanism to re-upload a part that fails to be uploaded.
    -   Increase the timeout period.
    ```
    // Code example of multipart upload in JS SDK
    
    let retryCount = 0;
    let retryCountMax = 3;
    ...
    const uploadFile = function uploadFile(client) {
      if (! uploadFileClient || Object.keys(uploadFileClient).length === 0) {
        uploadFileClient = client;
      }
      ...
      
      console.log(`${file.name} => ${key}`);
      const options = {
        progress,
        Partsize: 1000*1024, // Set the part size.
        Timeout: 120000, // Set the timeout period.
      };
      if (currentCheckpoint) {
        options.checkpoint = currentCheckpoint;
      }
      return uploadFileClient.multipartUpload(key, file, options).then((res) => {
        console.log('upload success: %j', res);
        currentCheckpoint = null;
        uploadFileClient = null;
      }).catch((err) => {
        if (uploadFileClient && uploadFileClient.isCancel()) {
          console.log('stop-upload!') ;
        } else {
          console.error(err);
          //retry
          if (retryCount < retryCountMax){
              retryCount++;
              console.error("retryCount : " + retryCount);
              uploadFile('');
          }
        }
      });
    };
    ```


## Summary {#section_ovc_lg2_qfb .section}

If you access OSS data with a standard OSS domain name \(for example, oss-cn-hangzhou.aliyuncs.com\), your access is implemented through the carrier network. In this case, a ConnectionTimeOut error may occur in uploads due to complex network environments \(such as unstable network or poor network conditions\). You can try the following solutions:

-   Upload files with the multipart upload method and limit the part size in a range from 100 KB to 1 MB.

    **Note:** The OSS server does not receive parts smaller than 100 KB.

-   Add a resumable mechanism to re-upload a part that fails to be uploaded.

    **Note:** The mechanism is enabled in Android/iOS SDK by default and therefore no configuration is required.

-   Increase the timeout period.
-   Use the CDN acceleration service to accelerate data transmission in OSS.

